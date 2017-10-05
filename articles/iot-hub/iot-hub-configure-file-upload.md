---
title: "Fájlfeltöltés konfigurálása az Azure-portál használatával |} Microsoft Docs"
description: "Hogyan használható az Azure-portálon az IoT hub engedélyezéséhez a csatlakoztatott eszközökből fájlfeltöltéseket konfigurálásához. A cél Azure storage-fiók konfigurálásával kapcsolatos információkat tartalmazza."
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
ms.openlocfilehash: 149dd84d7d8f4ff9cd30f9fc649ced3cb364cfb7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a><span data-ttu-id="7b7ac-104">Az Azure portál használatával fájlfeltöltések IoT-központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7b7ac-104">Configure IoT Hub file uploads using the Azure portal</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a><span data-ttu-id="7b7ac-105">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="7b7ac-105">File upload</span></span>

<span data-ttu-id="7b7ac-106">Használatához a [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure Storage-fiókot a központ.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-106">To use the [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your hub.</span></span> <span data-ttu-id="7b7ac-107">Válassza ki **fájlfeltöltés** és jelenítse meg a fájl feltöltése tulajdonságait az IoT hub, amely módosítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-107">Select **File upload** to display a list of file upload properties for the IoT hub that is being modified.</span></span>

![Az IoT-központ fájlfeltöltés a portál beállítások megtekintése][13]

<span data-ttu-id="7b7ac-109">**A tároló**: az Azure portál segítségével válassza ki a blob-tároló az IoT Hub társítani az aktuális Azure-előfizetéshez az Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-109">**Storage container**: Use the Azure portal to select a blob container in an Azure Storage account in your current Azure subscription to associate with your IoT Hub.</span></span> <span data-ttu-id="7b7ac-110">Ha szükséges, létrehozhat egy Azure Storage-fiókot a **tárfiókok** a panel megnyitásához, és a blob tároló a **tárolók** panelen.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-110">If necessary, you can create an Azure Storage account on the **Storage accounts** blade and blob container on the **Containers** blade.</span></span> <span data-ttu-id="7b7ac-111">Az IoT-központ automatikusan létrehozza a SAS URI-azonosítók eszközöket használja, ha ezek a fájlok feltöltése a blob tároló írási engedéllyel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-111">IoT Hub automatically generates SAS URIs with write permissions to this blob container for devices to use when they upload files.</span></span>

![A fájl feltöltése a tároló megtekintése a portálon][14]

<span data-ttu-id="7b7ac-113">**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések a váltógomb keresztül.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-113">**Receive notifications for uploaded files**: Enable or disable file upload notifications via the toggle.</span></span>

<span data-ttu-id="7b7ac-114">**SAS-élettartam**: Ez a beállítás akkor a idő élettartamát az az eszközt az IoT-központ által visszaadott SAS URI-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-114">**SAS TTL**: This setting is the time-to-live of the SAS URIs returned to the device by IoT Hub.</span></span> <span data-ttu-id="7b7ac-115">Alapértelmezés szerint egyórás beállítva, de más értékek, a csúszka segítségével testre szabható.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-115">Set to one hour by default but can be customized to other values using the slider.</span></span>

<span data-ttu-id="7b7ac-116">**Az értesítési beállítások alapértelmezett élettartam**: az idő TTL-fájl feltöltése értesítési, előtt lejárt.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-116">**File notification settings default TTL**: The time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="7b7ac-117">Alapértelmezés szerint egy nap beállítva, de más értékek, a csúszka segítségével testre szabható.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-117">Set to one day by default but can be customized to other values using the slider.</span></span>

<span data-ttu-id="7b7ac-118">**Értesítési maximális száma fájl**: A szám, ahányszor az IoT Hub megpróbál egy fájl feltöltése értesítést.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-118">**File notification maximum delivery count**: The number of times the IoT Hub attempts to deliver a file upload notification.</span></span> <span data-ttu-id="7b7ac-119">Alapértelmezés szerint 10-re állítva, de más értékek, a csúszka segítségével testre szabható.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-119">Set to 10 by default but can be customized to other values using the slider.</span></span>

![Konfigurálja a portálon az IoT Hub-fájl feltöltése][15]

## <a name="next-steps"></a><span data-ttu-id="7b7ac-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7b7ac-121">Next steps</span></span>

<span data-ttu-id="7b7ac-122">Az IoT-központ a fájl feltöltése képességeivel kapcsolatos további információk: [egy eszközről tölt fel] [ lnk-upload] az IoT Hub fejlesztői útmutató.</span><span class="sxs-lookup"><span data-stu-id="7b7ac-122">For more information about the file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload] in the IoT Hub developer guide.</span></span>

<span data-ttu-id="7b7ac-123">Az alábbi hivatkozásokból tudhat meg többet az Azure IoT Hub kezelése:</span><span class="sxs-lookup"><span data-stu-id="7b7ac-123">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="7b7ac-124">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="7b7ac-124">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="7b7ac-125">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="7b7ac-125">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="7b7ac-126">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="7b7ac-126">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="7b7ac-127">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="7b7ac-127">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="7b7ac-128">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="7b7ac-128">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="7b7ac-129">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="7b7ac-129">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="7b7ac-130">[Az IoT-megoldásból az alapoktól biztonságos mentése][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="7b7ac-130">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

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
