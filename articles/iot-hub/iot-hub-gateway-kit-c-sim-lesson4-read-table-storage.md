---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 4: Table storage |} Microsoft Docs"
description: "Intel NUC tooyour IoT hubról üzenetek menthetők, írja őket tooAzure a Table storage, és majd hello felhőből olvashatja őket."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok beolvasása a felhőből, iot-felhőszolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 78e4b6ea-968d-401e-a7dc-8f9acdb3ec1a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 43e299d63bbbe10d4d867af25e700c3a7cc07c53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="a0df5-104">Azure Table storage-ban tárolt üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="a0df5-104">Read messages persisted in Azure Table storage</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a0df5-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a0df5-105">What you will do</span></span>

- <span data-ttu-id="a0df5-106">Az átjáró által küldött üzenetek tooyour IoT-központ hello átjáró mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a0df5-106">Run hello gateway sample application on your gateway that sends messages tooyour IoT hub.</span></span>
- <span data-ttu-id="a0df5-107">Mintakód futtassa a fogadó számítógép tooread üzeneteket az Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="a0df5-107">Run sample code on your host computer tooread messages in your Azure Table storage.</span></span>

<span data-ttu-id="a0df5-108">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a0df5-108">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a0df5-109">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a0df5-109">What you will learn</span></span>

<span data-ttu-id="a0df5-110">Hogyan toouse hello gulp eszköz toorun minta kód tooread köszönőüzenetei az Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="a0df5-110">How toouse hello gulp tool toorun hello sample code tooread messages in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a0df5-111">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a0df5-111">What you need</span></span>

<span data-ttu-id="a0df5-112">Sikeres rendelkeznek hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="a0df5-112">You have have successfully done hello following tasks:</span></span>

- <span data-ttu-id="a0df5-113">[Hello Azure függvény alkalmazás és hello Azure storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="a0df5-113">[Created hello Azure function app and hello Azure storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span></span>
- <span data-ttu-id="a0df5-114">[Hello átjáró mintaalkalmazás futtatása](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="a0df5-114">[Run hello gateway sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>
- <span data-ttu-id="a0df5-115">[Üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span><span class="sxs-lookup"><span data-stu-id="a0df5-115">[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span></span>

## <a name="get-your-azure-storage-connection-strings"></a><span data-ttu-id="a0df5-116">Az Azure storage kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="a0df5-116">Get your Azure storage connection strings</span></span>

<span data-ttu-id="a0df5-117">Ez a lecke korai szakaszában sikeresen létrehozott egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="a0df5-117">Early in this lesson, you successfully created an Azure storage account.</span></span> <span data-ttu-id="a0df5-118">tooget hello kapcsolati karakterlánca hello Azure storage-fiók, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="a0df5-118">tooget hello connection string of hello Azure storage account, run hello following commands:</span></span>

* <span data-ttu-id="a0df5-119">A tárfiókok listázása.</span><span class="sxs-lookup"><span data-stu-id="a0df5-119">List all your storage accounts.</span></span>

```bash
az storage account list -g iot-gateway --query [].name
```

* <span data-ttu-id="a0df5-120">Az azure storage kapcsolati karakterlánc beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a0df5-120">Get azure storage connection string.</span></span>

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

<span data-ttu-id="a0df5-121">Használjon `iot-gateway` hello értékeként `{resource group name}` nem módosítása hello érték a 2.</span><span class="sxs-lookup"><span data-stu-id="a0df5-121">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="a0df5-122">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a0df5-122">Configure hello device connection</span></span>

<span data-ttu-id="a0df5-123">Frissítés hello `config-azure.json` fájlt úgy, hogy hello mintakód hello állomáson futó elolvashatják üzenet az Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="a0df5-123">Update hello `config-azure.json` file so that hello sample code that runs on hello host computer can read message in your Azure Table storage.</span></span> <span data-ttu-id="a0df5-124">tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a0df5-124">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="a0df5-125">Nyissa meg hello eszköz konfigurációs fájl `config-azure.json` hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a0df5-125">Open hello device configuration file `config-azure.json` by running hello following commands:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguráció](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. <span data-ttu-id="a0df5-127">Cserélje le `[Azure storage connection string]` a hello szerezte be az Azure storage kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a0df5-127">Replace `[Azure storage connection string]` with hello Azure storage connection string that you obtained.</span></span>

   <span data-ttu-id="a0df5-128">`[IoT hub connection string]`már helyébe szakaszban [üzenetek olvasásakor Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) a Lesson3.</span><span class="sxs-lookup"><span data-stu-id="a0df5-128">`[IoT hub connection string]` should already be replaced in section [Read messages from Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) in Lesson3.</span></span>

## <a name="read-messages-in-your-azure-table-storage"></a><span data-ttu-id="a0df5-129">Az Azure Table storage üzenet olvasása</span><span class="sxs-lookup"><span data-stu-id="a0df5-129">Read messages in your Azure Table storage</span></span>

<span data-ttu-id="a0df5-130">Hello átjáró mintaalkalmazás futtatása, és Azure Table storage üzenetek olvashatják hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a0df5-130">Run hello gateway sample application and read Azure Table storage messages by hello following command:</span></span>

```bash
gulp run --table-storage
```

<span data-ttu-id="a0df5-131">Az IoT hub új üzenet érkezésekor elindítja az Azure Table storage-be az Azure-függvény alkalmazás toosave üzenetet.</span><span class="sxs-lookup"><span data-stu-id="a0df5-131">Your IoT hub triggers your Azure Function application toosave message into your Azure Table storage when new message arrives.</span></span>
<span data-ttu-id="a0df5-132">Hello `gulp run` parancs által küldött üzenetek tooyour IoT-központ átjáró mintaalkalmazás futtatja.</span><span class="sxs-lookup"><span data-stu-id="a0df5-132">hello `gulp run` command runs gateway sample application that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="a0df5-133">A `table-storage` paraméter, akkor is indít folyamatot gyermek tooreceive hello üzenet mentése az Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="a0df5-133">With `table-storage` parameter, it also spawns a child process tooreceive hello saved message in your Azure Table storage.</span></span>

<span data-ttu-id="a0df5-134">küldött és fogadott azonos konzolablakában az összes megjelenített azonnal hello köszönőüzenetei hello gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="a0df5-134">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="a0df5-135">hello minta alkalmazáspéldány 40 másodperc múlva automatikusan leáll.</span><span class="sxs-lookup"><span data-stu-id="a0df5-135">hello sample application instance will terminate automatically in 40 seconds.</span></span>

   ![gulp olvasása](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table_simudev.png)


## <a name="summary"></a><span data-ttu-id="a0df5-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a0df5-137">Summary</span></span>

<span data-ttu-id="a0df5-138">Minta kód tooread hello köszönőüzenetei az Azure Table Storage mentése az Azure-függvény alkalmazás futtatását.</span><span class="sxs-lookup"><span data-stu-id="a0df5-138">You've run hello sample code tooread hello messages in your Azure Table storage saved by your Azure Function application.</span></span>
