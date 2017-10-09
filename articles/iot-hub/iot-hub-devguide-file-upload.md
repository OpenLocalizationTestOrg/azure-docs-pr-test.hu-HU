---
title: "Azure IoT Hub-fájl feltöltése aaaUnderstand |} Microsoft Docs"
description: "Fejlesztői útmutató – az eszköz tooan Azure tárolóra fájlok feltöltése az IoT-központ toomanage használata hello fájl feltöltése szolgáltatása."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a0427925-3e40-4fcd-96c1-2a31d1ddc14b
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: d44f9303ead4fa282dc0baf70887af1b8a03293d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-with-iot-hub"></a><span data-ttu-id="10ed0-103">Az IoT hubbal fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="10ed0-103">Upload files with IoT Hub</span></span>

<span data-ttu-id="10ed0-104">Hello részletezett [IoT-központok végpontjai] [ lnk-endpoints] a cikkben egy eszköz is kezdeményezhető a fájlfeltöltés úgy, hogy egy eszköz felé néző végpont keresztül értesítést küld (**/devices/ {deviceId} /fájlok**).</span><span class="sxs-lookup"><span data-stu-id="10ed0-104">As detailed in hello [IoT Hub endpoints][lnk-endpoints] article, a device can initiate a file upload by sending a notification through a device-facing endpoint (**/devices/{deviceId}/files**).</span></span> <span data-ttu-id="10ed0-105">Amikor egy eszköz értesíti, hogy befejeződött-e a feltöltés IoT Hub, az IoT-központ keresztül hello fájl feltöltése értesítő üzenetet küld a **/messages/servicebound/filenotifications** szolgáltatás felé néző végpont.</span><span class="sxs-lookup"><span data-stu-id="10ed0-105">When a device notifies IoT Hub that an upload is complete, IoT Hub sends a file upload notification message through hello **/messages/servicebound/filenotifications** service-facing endpoint.</span></span>

<span data-ttu-id="10ed0-106">Helyett kapcsolatoknak a üzeneteket maga az IoT-központ keresztül, az IoT-központ helyette eleget kell egy kézbesítő tooan társított Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="10ed0-106">Instead of brokering messages through IoT Hub itself, IoT Hub instead acts as a dispatcher tooan associated Azure Storage account.</span></span> <span data-ttu-id="10ed0-107">Egy eszköz kér, a tárolási jogkivonatot adott toohello fájl hello eszköz IoT-központ által tooupload.</span><span class="sxs-lookup"><span data-stu-id="10ed0-107">A device requests a storage token from IoT Hub that is specific toohello file hello device wishes tooupload.</span></span> <span data-ttu-id="10ed0-108">hello eszköz hello SAS URI tooupload hello fájl toostorage használja, és hello feltöltés befejezésekor hello eszköz befejezési tooIoT Hub, értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="10ed0-108">hello device uses hello SAS URI tooupload hello file toostorage, and when hello upload is complete hello device sends a notification of completion tooIoT Hub.</span></span> <span data-ttu-id="10ed0-109">Az IoT-központ hello fájl feltöltése befejeződött, és hozzáadja a fájl feltöltése értesítési üzenet toohello szolgáltatás irányuló fájl értesítési végpont ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="10ed0-109">IoT Hub checks hello file upload is complete and then adds a file upload notification message toohello service-facing file notification endpoint.</span></span>

<span data-ttu-id="10ed0-110">Mielőtt feltölti a fájl tooIoT Hub az eszközről, konfigurálnia kell a központ által [társítása egy Azure Storage] [ lnk-associate-storage] tooit fiók.</span><span class="sxs-lookup"><span data-stu-id="10ed0-110">Before you upload a file tooIoT Hub from a device, you must configure your hub by [associating an Azure Storage][lnk-associate-storage] account tooit.</span></span>

<span data-ttu-id="10ed0-111">Az eszköz lehet majd [feltöltés inicializálása] [ lnk-initialize] , majd [értesíteni az IoT-központ] [ lnk-notify] hello feltöltés befejezését.</span><span class="sxs-lookup"><span data-stu-id="10ed0-111">Your device can then [initialize an upload][lnk-initialize] and then [notify IoT hub][lnk-notify] when hello upload completes.</span></span> <span data-ttu-id="10ed0-112">Ha szükséges, ha egy eszköz értesíti az IoT-központ adott hello feltöltése befejeződött, hello szolgáltatást hozhat létre egy [értesítési üzenet][lnk-service-notification].</span><span class="sxs-lookup"><span data-stu-id="10ed0-112">Optionally, when a device notifies IoT Hub that hello upload is complete, hello service can generate a [notification message][lnk-service-notification].</span></span>

### <a name="when-toouse"></a><span data-ttu-id="10ed0-113">Ha toouse</span><span class="sxs-lookup"><span data-stu-id="10ed0-113">When toouse</span></span>

<span data-ttu-id="10ed0-114">Fájl feltöltése toosend médiafájlok és nagy telemetriai kötegek töltötte fel időnként csatlakoztatott eszközök vagy tömörített toosave sávszélesség használata.</span><span class="sxs-lookup"><span data-stu-id="10ed0-114">Use file upload toosend media files and large telemetry batches uploaded by intermittently connected devices or compressed toosave bandwidth.</span></span>

<span data-ttu-id="10ed0-115">Tekintse meg a túl[eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] Ha bizonytalan jelentett tulajdonságok, az eszköz a felhőbe küldött üzeneteket vagy a fájl feltöltése használata között.</span><span class="sxs-lookup"><span data-stu-id="10ed0-115">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using reported properties, device-to-cloud messages, or file upload.</span></span>

## <a name="associate-an-azure-storage-account-with-iot-hub"></a><span data-ttu-id="10ed0-116">Egy Azure Storage-fiók társítása az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="10ed0-116">Associate an Azure Storage account with IoT Hub</span></span>

<span data-ttu-id="10ed0-117">toouse hello fájl feltöltése funkciót, először egy Azure Storage-fiók toohello IoT-központ rendelnie.</span><span class="sxs-lookup"><span data-stu-id="10ed0-117">toouse hello file upload functionality, you must first link an Azure Storage account toohello IoT Hub.</span></span> <span data-ttu-id="10ed0-118">Ez a feladat vagy hello hajthatja végre [Azure-portálon][lnk-management-portal], akár programozott módon hello [IoT-központ erőforrás-szolgáltató REST API-k] [ lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="10ed0-118">You can complete this task either through hello [Azure portal][lnk-management-portal], or programmatically through hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="10ed0-119">Amennyiben az Azure Storage-fiók társított az IoT Hub, hello szolgáltatás esetén ad vissza egy SAS URI tooa eszköz hello eszköz kéréssel fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="10ed0-119">Once you have associated an Azure Storage account with your IoT Hub, hello service returns a SAS URI tooa device when hello device initiates a file upload request.</span></span>

> [!NOTE]
> <span data-ttu-id="10ed0-120">Hello [Azure IoT SDK-k] [ lnk-sdks] automatikusan hello SAS URI-t, hello fájl feltöltése, és a feltöltött az IoT-központ értesítés leíró lekérése során.</span><span class="sxs-lookup"><span data-stu-id="10ed0-120">hello [Azure IoT SDKs][lnk-sdks] automatically handle retrieving hello SAS URI, uploading hello file, and notifying IoT Hub of a completed upload.</span></span>


## <a name="initialize-a-file-upload"></a><span data-ttu-id="10ed0-121">A fájlfeltöltés inicializálása</span><span class="sxs-lookup"><span data-stu-id="10ed0-121">Initialize a file upload</span></span>
<span data-ttu-id="10ed0-122">Az IoT-központ rendelkezik végpont kifejezetten eszközök toorequest tárolási tooupload egy fájlt egy SAS URI-Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="10ed0-122">IoT Hub has an endpoint specifically for devices toorequest a SAS URI for storage tooupload a file.</span></span> <span data-ttu-id="10ed0-123">tooinitiate hello fájl feltöltési folyamat hello eszköz egy POST kérést küld túl`{iot hub}.azure-devices.net/devices/{deviceId}/files` a hello a következő JSON-törzsére:</span><span class="sxs-lookup"><span data-stu-id="10ed0-123">tooinitiate hello file upload process, hello device sends a POST request too`{iot hub}.azure-devices.net/devices/{deviceId}/files` with hello following JSON body:</span></span>

```json
{
    "blobName": "{name of hello file for which a SAS URI will be generated}"
}
```

<span data-ttu-id="10ed0-124">Az IoT-központ ad vissza adatokat, hogy hello eszközök tooupload hello fájlt használja a következő hello:</span><span class="sxs-lookup"><span data-stu-id="10ed0-124">IoT Hub returns hello following data, which hello device uses tooupload hello file:</span></span>

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a><span data-ttu-id="10ed0-125">Elavult: a GET fájlfeltöltés inicializálása</span><span class="sxs-lookup"><span data-stu-id="10ed0-125">Deprecated: initialize a file upload with a GET</span></span>

> [!NOTE]
> <span data-ttu-id="10ed0-126">Ez a szakasz ismerteti, hogyan elavult funkciók tooreceive az IoT-központ SAS URI.</span><span class="sxs-lookup"><span data-stu-id="10ed0-126">This section describes deprecated functionality for how tooreceive a SAS URI from IoT Hub.</span></span> <span data-ttu-id="10ed0-127">A fentiekben ismertetett hello POST metódussal használható.</span><span class="sxs-lookup"><span data-stu-id="10ed0-127">Use hello POST method described previously.</span></span>

<span data-ttu-id="10ed0-128">Az IoT-központ két REST végpontok toosupport fájl feltöltéséhez, egy tooget hello SAS URI-t a tároláshoz, és más toonotify hello IoT-központ, a feltöltött hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="10ed0-128">IoT Hub has two REST endpoints toosupport file upload, one tooget hello SAS URI for storage and hello other toonotify hello IoT hub of a completed upload.</span></span> <span data-ttu-id="10ed0-129">hello eszköz kezdeményezi hello fájl feltöltési folyamat egy GET toohello IoT-központot, elküldésével `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span><span class="sxs-lookup"><span data-stu-id="10ed0-129">hello device initiates hello file upload process by sending a GET toohello IoT hub at `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span></span> <span data-ttu-id="10ed0-130">az IoT-központ hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="10ed0-130">hello IoT hub returns:</span></span>

* <span data-ttu-id="10ed0-131">Egy adott toohello fájl toobe feltöltött SAS URI.</span><span class="sxs-lookup"><span data-stu-id="10ed0-131">A SAS URI specific toohello file toobe uploaded.</span></span>
* <span data-ttu-id="10ed0-132">Amikor befejeződött a feltöltés hello használt korrelációs azonosító toobe.</span><span class="sxs-lookup"><span data-stu-id="10ed0-132">A correlation ID toobe used once hello upload is completed.</span></span>

## <a name="notify-iot-hub-of-a-completed-file-upload"></a><span data-ttu-id="10ed0-133">Az IoT-központ a befejezett fájlfeltöltés értesítése</span><span class="sxs-lookup"><span data-stu-id="10ed0-133">Notify IoT Hub of a completed file upload</span></span>

<span data-ttu-id="10ed0-134">hello eszköz felelős feltöltése hello fájl toostorage hello Azure Storage SDK-k használatával.</span><span class="sxs-lookup"><span data-stu-id="10ed0-134">hello device is responsible for uploading hello file toostorage using hello Azure Storage SDKs.</span></span> <span data-ttu-id="10ed0-135">Hello feltöltés befejeződése után hello eszköz egy POST kérést küld túl`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` a hello a következő JSON-törzsére:</span><span class="sxs-lookup"><span data-stu-id="10ed0-135">When hello upload is complete, hello device sends a POST request too`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` with hello following JSON body:</span></span>

```json
{
    "correlationId": "{correlation ID received from hello initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

<span data-ttu-id="10ed0-136">hello értékének `isSuccess` van egy logikai jelző, hogy hello fájl feltöltése sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="10ed0-136">hello value of `isSuccess` is a Boolean representing whether hello file was uploaded successfully.</span></span> <span data-ttu-id="10ed0-137">Az állapotkód: hello `statusCode` hello hello fájl toostorage hello feltöltésének állapotát, és hello `statusDescription` toohello megfelel `statusCode`.</span><span class="sxs-lookup"><span data-stu-id="10ed0-137">hello status code for `statusCode` is hello status for hello upload of hello file toostorage, and hello `statusDescription` corresponds toohello `statusCode`.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="10ed0-138">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="10ed0-138">Reference topics:</span></span>

<span data-ttu-id="10ed0-139">hello következő referencia-témakörökre nyújtanak további információt az eszközről fájlok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="10ed0-139">hello following reference topics provide you with more information about uploading files from a device.</span></span>

## <a name="file-upload-notifications"></a><span data-ttu-id="10ed0-140">Fájl feltöltése értesítések</span><span class="sxs-lookup"><span data-stu-id="10ed0-140">File upload notifications</span></span>

<span data-ttu-id="10ed0-141">Nem kötelező amikor egy eszköz értesíti az IoT-központ, hogy befejeződött-e a feltöltés, IoT-központ hozhat létre egy értesítési üzenetet, amely tartalmazza a hello fájl hello nevét és a tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="10ed0-141">Optionally, when a device notifies IoT Hub that an upload is complete, IoT Hub can generate a notification message that contains hello name and storage location of hello file.</span></span>

<span data-ttu-id="10ed0-142">A [végpontok][lnk-endpoints], IoT-központ biztosítja a fájl feltöltése értesítések keresztül a szolgáltatás felé néző végpont (**/messages/servicebound/fileuploadnotifications**) üzeneteihez.</span><span class="sxs-lookup"><span data-stu-id="10ed0-142">As explained in [Endpoints][lnk-endpoints], IoT Hub delivers file upload notifications through a service-facing endpoint (**/messages/servicebound/fileuploadnotifications**) as messages.</span></span> <span data-ttu-id="10ed0-143">hello szemantikáját kap, a fájl feltöltése értesítések hello ugyanaz, mint a felhő-eszközre küldött üzenetek és hello azonos [üzenet életciklus][lnk-lifecycle].</span><span class="sxs-lookup"><span data-stu-id="10ed0-143">hello receive semantics for file upload notifications are hello same as for cloud-to-device messages and have hello same [message lifecycle][lnk-lifecycle].</span></span> <span data-ttu-id="10ed0-144">Minden egyes hello fájl feltöltése értesítési végpont lekért üzenet egy JSON rekordot hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="10ed0-144">Each message retrieved from hello file upload notification endpoint is a JSON record with hello following properties:</span></span>

| <span data-ttu-id="10ed0-145">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="10ed0-145">Property</span></span> | <span data-ttu-id="10ed0-146">Leírás</span><span class="sxs-lookup"><span data-stu-id="10ed0-146">Description</span></span> |
| --- | --- |
| <span data-ttu-id="10ed0-147">EnqueuedTimeUtc</span><span class="sxs-lookup"><span data-stu-id="10ed0-147">EnqueuedTimeUtc</span></span> |<span data-ttu-id="10ed0-148">Hello értesítés létrehozásának jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="10ed0-148">Timestamp indicating when hello notification was created.</span></span> |
| <span data-ttu-id="10ed0-149">Eszközazonosító</span><span class="sxs-lookup"><span data-stu-id="10ed0-149">DeviceId</span></span> |<span data-ttu-id="10ed0-150">**DeviceId** hello eszköz, amely hello fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="10ed0-150">**DeviceId** of hello device which uploaded hello file.</span></span> |
| <span data-ttu-id="10ed0-151">BlobUri</span><span class="sxs-lookup"><span data-stu-id="10ed0-151">BlobUri</span></span> |<span data-ttu-id="10ed0-152">URI-je hello feltöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="10ed0-152">URI of hello uploaded file.</span></span> |
| <span data-ttu-id="10ed0-153">Blobnév</span><span class="sxs-lookup"><span data-stu-id="10ed0-153">BlobName</span></span> |<span data-ttu-id="10ed0-154">Hello neve feltöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="10ed0-154">Name of hello uploaded file.</span></span> |
| <span data-ttu-id="10ed0-155">LastUpdatedTime</span><span class="sxs-lookup"><span data-stu-id="10ed0-155">LastUpdatedTime</span></span> |<span data-ttu-id="10ed0-156">Hello fájl utolsó frissítésekor jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="10ed0-156">Timestamp indicating when hello file was last updated.</span></span> |
| <span data-ttu-id="10ed0-157">BlobSizeInBytes</span><span class="sxs-lookup"><span data-stu-id="10ed0-157">BlobSizeInBytes</span></span> |<span data-ttu-id="10ed0-158">Hello mérete feltöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="10ed0-158">Size of hello uploaded file.</span></span> |

<span data-ttu-id="10ed0-159">**Példa**.</span><span class="sxs-lookup"><span data-stu-id="10ed0-159">**Example**.</span></span> <span data-ttu-id="10ed0-160">Ez a példa bemutatja, hogy egy fájl hello törzsében feltöltése értesítési üzenetet.</span><span class="sxs-lookup"><span data-stu-id="10ed0-160">This example shows hello body of a file upload notification message.</span></span>

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a><span data-ttu-id="10ed0-161">Fájl feltöltése értesítési konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="10ed0-161">File upload notification configuration options</span></span>

<span data-ttu-id="10ed0-162">Minden egyes IoT-központ elérhetővé teszi az alábbi konfigurációs beállítások, a fájl feltöltése értesítések hello:</span><span class="sxs-lookup"><span data-stu-id="10ed0-162">Each IoT hub exposes hello following configuration options for file upload notifications:</span></span>

| <span data-ttu-id="10ed0-163">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="10ed0-163">Property</span></span> | <span data-ttu-id="10ed0-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="10ed0-164">Description</span></span> | <span data-ttu-id="10ed0-165">Tartomány- és alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="10ed0-165">Range and default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="10ed0-166">**enableFileUploadNotifications**</span><span class="sxs-lookup"><span data-stu-id="10ed0-166">**enableFileUploadNotifications**</span></span> |<span data-ttu-id="10ed0-167">Meghatározza, hogy fájl feltöltése értesítések írt toohello fájl értesítések végpont.</span><span class="sxs-lookup"><span data-stu-id="10ed0-167">Controls whether file upload notifications are written toohello file notifications endpoint.</span></span> |<span data-ttu-id="10ed0-168">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="10ed0-168">Bool.</span></span> <span data-ttu-id="10ed0-169">Alapértelmezett: igaz.</span><span class="sxs-lookup"><span data-stu-id="10ed0-169">Default: True.</span></span> |
| <span data-ttu-id="10ed0-170">**fileNotifications.ttlAsIso8601**</span><span class="sxs-lookup"><span data-stu-id="10ed0-170">**fileNotifications.ttlAsIso8601**</span></span> |<span data-ttu-id="10ed0-171">Alapértelmezett élettartam-fájl feltöltése értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="10ed0-171">Default TTL for file upload notifications.</span></span> |<span data-ttu-id="10ed0-172">Másolatot too48H ISO_8601 időköz (legalább 1 perc).</span><span class="sxs-lookup"><span data-stu-id="10ed0-172">ISO_8601 interval up too48H (minimum 1 minute).</span></span> <span data-ttu-id="10ed0-173">Alapértelmezett: 1 óra.</span><span class="sxs-lookup"><span data-stu-id="10ed0-173">Default: 1 hour.</span></span> |
| <span data-ttu-id="10ed0-174">**fileNotifications.lockDuration**</span><span class="sxs-lookup"><span data-stu-id="10ed0-174">**fileNotifications.lockDuration**</span></span> |<span data-ttu-id="10ed0-175">Hello fájl feltöltése értesítések várólista zárolási időtartama.</span><span class="sxs-lookup"><span data-stu-id="10ed0-175">Lock duration for hello file upload notifications queue.</span></span> |<span data-ttu-id="10ed0-176">5 too300 másodpercben (legalább 5 másodperces).</span><span class="sxs-lookup"><span data-stu-id="10ed0-176">5 too300 seconds (minimum 5 seconds).</span></span> <span data-ttu-id="10ed0-177">Alapértelmezett: 60 másodperc.</span><span class="sxs-lookup"><span data-stu-id="10ed0-177">Default: 60 seconds.</span></span> |
| <span data-ttu-id="10ed0-178">**fileNotifications.maxDeliveryCount**</span><span class="sxs-lookup"><span data-stu-id="10ed0-178">**fileNotifications.maxDeliveryCount**</span></span> |<span data-ttu-id="10ed0-179">Hello fájl maximális száma töltse fel az értesítési várólista.</span><span class="sxs-lookup"><span data-stu-id="10ed0-179">Maximum delivery count for hello file upload notification queue.</span></span> |<span data-ttu-id="10ed0-180">1 too100.</span><span class="sxs-lookup"><span data-stu-id="10ed0-180">1 too100.</span></span> <span data-ttu-id="10ed0-181">Alapértelmezett: 100.</span><span class="sxs-lookup"><span data-stu-id="10ed0-181">Default: 100.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="10ed0-182">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="10ed0-182">Additional reference material</span></span>

<span data-ttu-id="10ed0-183">Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="10ed0-183">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="10ed0-184">[IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.</span><span class="sxs-lookup"><span data-stu-id="10ed0-184">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="10ed0-185">[Sávszélesség-szabályozási és kvóták] [ lnk-quotas] hello kvóták ismerteti, és a szabályozás viselkedéseket, amelyek az IoT-központ szolgáltatás toohello alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="10ed0-185">[Throttling and quotas][lnk-quotas] describes hello quotas and throttling behaviors that apply toohello IoT Hub service.</span></span>
* <span data-ttu-id="10ed0-186">[Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.</span><span class="sxs-lookup"><span data-stu-id="10ed0-186">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="10ed0-187">[Az IoT-központ lekérdezési nyelv] [ lnk-query] hello lekérdezési nyelv használhatja tooretrieve IoT Hub-ből származó adataival a eszköz twins és a feladatokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="10ed0-187">[IoT Hub query language][lnk-query] describes hello query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="10ed0-188">[Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="10ed0-188">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10ed0-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10ed0-189">Next steps</span></span>

<span data-ttu-id="10ed0-190">Most megtanulta, hogyan tooupload eszközök IoT-központ a fájlokat, esetleg érdekli hello IoT Hub fejlesztői útmutató témakörei a következő:</span><span class="sxs-lookup"><span data-stu-id="10ed0-190">Now you have learned how tooupload files from devices using IoT Hub, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="10ed0-191">[Az IoT Hub eszköz identitásainak kezelése][lnk-devguide-identities]</span><span class="sxs-lookup"><span data-stu-id="10ed0-191">[Manage device identities in IoT Hub][lnk-devguide-identities]</span></span>
* <span data-ttu-id="10ed0-192">[Központi hozzáférési tooIoT szabályozása][lnk-devguide-security]</span><span class="sxs-lookup"><span data-stu-id="10ed0-192">[Control access tooIoT Hub][lnk-devguide-security]</span></span>
* <span data-ttu-id="10ed0-193">[Az eszköz twins toosynchronize állapotát és konfigurációkat használják][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="10ed0-193">[Use device twins toosynchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="10ed0-194">[Az eszközön közvetlen metódus][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="10ed0-194">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="10ed0-195">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="10ed0-195">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="10ed0-196">Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ az oktatóanyag következő hello iránt érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="10ed0-196">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="10ed0-197">[Hogyan eszközök toohello tooupload fájlok cloud Az IoT hubbal][lnk-fileupload-tutorial]</span><span class="sxs-lookup"><span data-stu-id="10ed0-197">[How tooupload files from devices toohello cloud with IoT Hub][lnk-fileupload-tutorial]</span></span>

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
