---
title: "SensorTag eszköz & Azure IoT átjáró - rész 4: Table storage |} Microsoft Docs"
description: "Intel NUC üzenetek mentéséhez az IoT hub, Azure Table Storage írja őket, és majd olvashatja őket a felhőből."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok beolvasása a felhőből, iot-felhőszolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 8ca78045-ad92-4a6a-90f1-05f9668e6f0e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 72659ef3a7fd2f6011590d37176fd05503269aff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="7fb26-104">Azure Table storage-ban tárolt üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="7fb26-104">Read messages persisted in Azure Table storage</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="7fb26-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="7fb26-105">What you will do</span></span>

- <span data-ttu-id="7fb26-106">Futtassa az átjáró mintaalkalmazást az átjárón, amely üzeneteket küld az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="7fb26-106">Run the gateway sample application on your gateway that sends messages to your IoT hub.</span></span>
- <span data-ttu-id="7fb26-107">Futtassa egy mintakódot az Azure Table storage-ban üzeneteket beolvasni a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="7fb26-107">Then run a sample code on your host computer to read the messages in your Azure Table storage.</span></span> 

<span data-ttu-id="7fb26-108">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7fb26-108">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="7fb26-109">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="7fb26-109">What you will learn</span></span>

<span data-ttu-id="7fb26-110">Hogyan használhatja a gulp eszközt az Azure Table storage üzeneteit mintakódot futtatásához.</span><span class="sxs-lookup"><span data-stu-id="7fb26-110">How to use the gulp tool to run the sample code to read messages in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="7fb26-111">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="7fb26-111">What you need</span></span>

<span data-ttu-id="7fb26-112">Sikeres rendelkeznek kész a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="7fb26-112">You have have successfully done the following tasks:</span></span>

- <span data-ttu-id="7fb26-113">[Az Azure-függvény alkalmazás és az Azure storage-fiók létrehozása](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="7fb26-113">[Created the Azure function app and the Azure storage account](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).</span></span>
- <span data-ttu-id="7fb26-114">[Futtassa az átjáró mintaalkalmazást](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md).</span><span class="sxs-lookup"><span data-stu-id="7fb26-114">[Run the gateway sample application](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md).</span></span>
- <span data-ttu-id="7fb26-115">[Üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).</span><span class="sxs-lookup"><span data-stu-id="7fb26-115">[Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).</span></span>

## <a name="get-your-azure-storage-connection-strings"></a><span data-ttu-id="7fb26-116">Az Azure storage kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="7fb26-116">Get your Azure storage connection strings</span></span>

<span data-ttu-id="7fb26-117">Ez a lecke korai szakaszában sikeresen létrehozott egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="7fb26-117">Early in this lesson, you successfully created an Azure storage account.</span></span> <span data-ttu-id="7fb26-118">Ahhoz, hogy az Azure-tárfiók kapcsolati karakterlánca, a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="7fb26-118">To get the connection string of the Azure storage account, run the following commands:</span></span>

* <span data-ttu-id="7fb26-119">A tárfiókok listázása.</span><span class="sxs-lookup"><span data-stu-id="7fb26-119">List all your storage accounts.</span></span>

```bash
az storage account list -g iot-gateway --query [].name
```

* <span data-ttu-id="7fb26-120">Az azure storage kapcsolati karakterlánc beolvasása.</span><span class="sxs-lookup"><span data-stu-id="7fb26-120">Get azure storage connection string.</span></span>

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

<span data-ttu-id="7fb26-121">Az iot-átjáró használatához értékeként `{resource group name}` Ha nem módosítja az érték a 2.</span><span class="sxs-lookup"><span data-stu-id="7fb26-121">Use iot-gateway as the value of `{resource group name}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="7fb26-122">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7fb26-122">Configure the device connection</span></span>

<span data-ttu-id="7fb26-123">Frissítés a `config-azure.json` fájlt úgy, hogy a mintakódot az állomáson futó elolvashatják üzenet az Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="7fb26-123">Update the `config-azure.json` file so that the sample code that runs on the host computer can read message in your Azure Table storage.</span></span> <span data-ttu-id="7fb26-124">Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7fb26-124">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="7fb26-125">Nyissa meg az eszköz konfigurációs fájlját `config-azure.json` a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7fb26-125">Open the device configuration file `config-azure.json` by running the following commands:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguráció](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. <span data-ttu-id="7fb26-127">Cserélje le `[Azure storage connection string]` szerezte be az Azure storage kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="7fb26-127">Replace `[Azure storage connection string]` with the Azure storage connection string that you obtained.</span></span>

   <span data-ttu-id="7fb26-128">`[IoT hub connection string]`már helyébe szakaszban [üzenetek olvasásakor Azure IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) a Lesson3.</span><span class="sxs-lookup"><span data-stu-id="7fb26-128">`[IoT hub connection string]` should already be replaced in section [Read messages from Azure IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) in Lesson3.</span></span>

## <a name="read-messages-in-your-azure-table-storage"></a><span data-ttu-id="7fb26-129">Az Azure Table storage üzenet olvasása</span><span class="sxs-lookup"><span data-stu-id="7fb26-129">Read messages in your Azure Table storage</span></span>

<span data-ttu-id="7fb26-130">Futtassa az átjáró mintaalkalmazást, és olvassa el az Azure Table storage üzenetek által a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7fb26-130">Run the gateway sample application and read Azure Table storage messages by the following command:</span></span>

```bash
gulp run --table-storage
```

<span data-ttu-id="7fb26-131">Az IoT hub elindítja az Azure-függvény alkalmazás mentse az üzenet az Azure Table storage az új üzenet érkezésekor.</span><span class="sxs-lookup"><span data-stu-id="7fb26-131">Your IoT hub triggers your Azure Function application to save message into your Azure Table storage when new message arrives.</span></span>
<span data-ttu-id="7fb26-132">A `gulp run` parancs fut, amely üzeneteket küld az IoT hub átjáró mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7fb26-132">The `gulp run` command runs gateway sample application that sends messages to your IoT hub.</span></span> <span data-ttu-id="7fb26-133">A `table-storage` paraméter, akkor is indít olyan alárendelt folyamatot, amely a mentett üzenet az Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="7fb26-133">With `table-storage` parameter, it also spawns a child process to receive the saved message in your Azure Table storage.</span></span>

<span data-ttu-id="7fb26-134">Által küldött és fogadott összes üzenetek azonnal jelenik meg az azonos konzolablak a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="7fb26-134">The messages that are being sent and received are all displayed instantly on the same console window in the host machine.</span></span> <span data-ttu-id="7fb26-135">A minta alkalmazáspéldány 40 másodperc múlva automatikusan leáll.</span><span class="sxs-lookup"><span data-stu-id="7fb26-135">The sample application instance will terminate automatically in 40 seconds.</span></span>

   ![gulp olvasása](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table.png)


## <a name="summary"></a><span data-ttu-id="7fb26-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="7fb26-137">Summary</span></span>

<span data-ttu-id="7fb26-138">Az Azure Table storage az Azure-függvény alkalmazás által mentett olvashatja a mintakódot futtatását.</span><span class="sxs-lookup"><span data-stu-id="7fb26-138">You've run the sample code to read the messages in your Azure Table storage saved by your Azure Function application.</span></span>