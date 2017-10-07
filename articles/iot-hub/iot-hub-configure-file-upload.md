---
title: "aaaUse hello Azure portál tooconfigure fájlfeltöltés |} Microsoft Docs"
description: "Hogyan toouse hello Azure portál tooconfigure a IoT hub tooenable fájlfeltöltések csatlakoztatott eszközökről. Hello cél Azure storage-fiók konfigurálásával kapcsolatos információkat tartalmazza."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: b90c3fbed47b4eb144d3cb7480068b7cfc776ba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-hello-azure-portal"></a><span data-ttu-id="474df-104">Konfigurálja az IoT-központ fájlfeltöltéseket hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="474df-104">Configure IoT Hub file uploads using hello Azure portal</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a><span data-ttu-id="474df-105">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="474df-105">File upload</span></span>

<span data-ttu-id="474df-106">toouse hello [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure Storage-fiókot a központ.</span><span class="sxs-lookup"><span data-stu-id="474df-106">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your hub.</span></span> <span data-ttu-id="474df-107">Válassza ki **fájlfeltöltés** toodisplay a fájl feltöltése tulajdonságlistát hello IoT hub, amely módosítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="474df-107">Select **File upload** toodisplay a list of file upload properties for hello IoT hub that is being modified.</span></span>

![Az IoT-központ fájlfeltöltés hello portál beállítások megtekintése][13]

<span data-ttu-id="474df-109">**A tároló**: hello Azure portál tooselect egy blob tároló, az a jelenlegi Azure-előfizetés tooassociate az IoT hubbal egy Azure Storage-fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="474df-109">**Storage container**: Use hello Azure portal tooselect a blob container in an Azure Storage account in your current Azure subscription tooassociate with your IoT Hub.</span></span> <span data-ttu-id="474df-110">Ha szükséges, létrehozhat egy Azure Storage-fiók hello **tárfiókok** panel megnyitásához, és a blob tároló a hello **tárolók** panelen.</span><span class="sxs-lookup"><span data-stu-id="474df-110">If necessary, you can create an Azure Storage account on hello **Storage accounts** blade and blob container on hello **Containers** blade.</span></span> <span data-ttu-id="474df-111">Ezek a fájlok feltöltése az IoT-központ automatikusan létrehozza SAS URI-azonosítók az írási engedélyek toothis blob tároló eszközök toouse.</span><span class="sxs-lookup"><span data-stu-id="474df-111">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

![Tekintse meg a fájl feltöltése a tároló hello portál][14]

<span data-ttu-id="474df-113">**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések hello váltása keresztül.</span><span class="sxs-lookup"><span data-stu-id="474df-113">**Receive notifications for uploaded files**: Enable or disable file upload notifications via hello toggle.</span></span>

<span data-ttu-id="474df-114">**SAS-élettartam**: Ez a beállítás akkor idő élettartamát a SAS URI-azonosítók toohello eszköz vissza az IoT-központ hello hello.</span><span class="sxs-lookup"><span data-stu-id="474df-114">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="474df-115">Alapértelmezés szerint beállított tooone óra, de is testre szabott tooother értékeket használja hello csúszkát.</span><span class="sxs-lookup"><span data-stu-id="474df-115">Set tooone hour by default but can be customized tooother values using hello slider.</span></span>

<span data-ttu-id="474df-116">**Az értesítési beállítások alapértelmezett élettartam**: hello idő TTL fájl feltöltése értesítés előtt lejárt.</span><span class="sxs-lookup"><span data-stu-id="474df-116">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="474df-117">Alapértelmezés szerint beállított tooone nap, de is testre szabott tooother értékeket használja hello csúszkát.</span><span class="sxs-lookup"><span data-stu-id="474df-117">Set tooone day by default but can be customized tooother values using hello slider.</span></span>

<span data-ttu-id="474df-118">**Értesítési maximális száma fájl**: hello számú alkalommal fordult elő az IoT-központ kísérletek toodeliver fájl feltöltése értesítést hello.</span><span class="sxs-lookup"><span data-stu-id="474df-118">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="474df-119">Alapértelmezés szerint beállított too10, de is testre szabott tooother értékeket használja hello csúszkát.</span><span class="sxs-lookup"><span data-stu-id="474df-119">Set too10 by default but can be customized tooother values using hello slider.</span></span>

![IoT Hub-fájl feltöltése hello portál konfigurálása][15]

## <a name="next-steps"></a><span data-ttu-id="474df-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="474df-121">Next steps</span></span>

<span data-ttu-id="474df-122">Az IoT-központ hello fájl feltöltése képességeivel kapcsolatos további információkért lásd: [egy eszközről tölt fel] [ lnk-upload] az IoT Hub fejlesztői útmutató hello.</span><span class="sxs-lookup"><span data-stu-id="474df-122">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload] in hello IoT Hub developer guide.</span></span>

<span data-ttu-id="474df-123">Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:</span><span class="sxs-lookup"><span data-stu-id="474df-123">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="474df-124">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="474df-124">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="474df-125">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="474df-125">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="474df-126">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="474df-126">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="474df-127">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="474df-127">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="474df-128">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="474df-128">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="474df-129">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="474df-129">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="474df-130">[Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="474df-130">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
