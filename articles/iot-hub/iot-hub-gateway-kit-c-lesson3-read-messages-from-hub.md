---
title: "SensorTag eszköz & Azure IoT átjáró - rész 3: üzeneteit |} Microsoft Docs"
description: "Futtassa a mintakódot az üzenetek olvasásakor az IoT hub a gazdaszámítógépen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok adatok"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cc88be24-b5c0-4ef2-ba21-4e8f77f3e167
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 45f3595c4848d5c283cdf95604adf8d2c8d6a809
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="88cbd-104">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="88cbd-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="88cbd-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="88cbd-105">What you will do</span></span>

- <span data-ttu-id="88cbd-106">Futtassa a mintakódot az IoT hub üzenetek olvasásakor a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="88cbd-106">Run sample code on your host computer to read messages from your IoT hub.</span></span>

<span data-ttu-id="88cbd-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="88cbd-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="88cbd-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="88cbd-108">What you will learn</span></span>

<span data-ttu-id="88cbd-109">Hogyan használhatja a gulp eszközt az IoT hub üzeneteket beolvasni.</span><span class="sxs-lookup"><span data-stu-id="88cbd-109">How to use the gulp tool to read messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="88cbd-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="88cbd-110">What you need</span></span>

- <span data-ttu-id="88cbd-111">A táblázat mintaalkalmazás, amely a 3 sikeresen lefutott.</span><span class="sxs-lookup"><span data-stu-id="88cbd-111">The BLE sample application that you ran successfully in Lesson 3.</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="88cbd-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="88cbd-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="88cbd-113">Az eszköz kapcsolati karakterlánc az eszköz (TI SensorTag vagy szimulált eszköz) az IoT hub való csatlakozáshoz használják.</span><span class="sxs-lookup"><span data-stu-id="88cbd-113">The device connection string is used by your device (TI SensorTag or simulated device) to connect to your IoT hub.</span></span> <span data-ttu-id="88cbd-114">Az IoT hub kapcsolati karakterlánc biztosít az identitásjegyzékhez az IoT hub kezelheti az eszközöket, amelyek használata engedélyezett az IoT hub csatlakozni a csatlakozáshoz használt.</span><span class="sxs-lookup"><span data-stu-id="88cbd-114">The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub.</span></span>

- <span data-ttu-id="88cbd-115">Az erőforráscsoport az IoT hub listán a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="88cbd-115">List all your IoT hubs in your resource group by running the following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="88cbd-116">Használjon `iot-gateway` értékeként `{resource group name}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="88cbd-116">Use `iot-gateway` as the value of `{resource group name}` if you didn't change the value.</span></span>
- <span data-ttu-id="88cbd-117">Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="88cbd-117">Get the IoT hub connection string by running the following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="88cbd-118">`{my hub name}`a rendszer az Ön által megadott nevét a 2.</span><span class="sxs-lookup"><span data-stu-id="88cbd-118">`{my hub name}` is the name that you specified in Lesson 2.</span></span>

## <a name="configure-the-device-connection-for-the-sample-code"></a><span data-ttu-id="88cbd-119">A mintakód az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="88cbd-119">Configure the device connection for the sample code</span></span>

<span data-ttu-id="88cbd-120">Az eszköz konfigurációs fájljának frissítése `config-azure.json` , hogy elolvashatja az IoT hub a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="88cbd-120">Update the device configuration file `config-azure.json` so that you can read messages from your IoT hub on your host computer.</span></span> <span data-ttu-id="88cbd-121">Ehhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="88cbd-121">To do this, follow these steps:</span></span>

1. <span data-ttu-id="88cbd-122">Nyissa meg `config-azure.json` a Visual Studio Code a konzolablakban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="88cbd-122">Open `config-azure.json` in Visual Studio Code by running the following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="88cbd-123">Ellenőrizze a következő cserékhez a `config-azure.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="88cbd-123">Make the following replacements in the `config-azure.json` file:</span></span>

   ![az azure config képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="88cbd-125">Cserélje le `[IoT hub connection string]` az IoT hub kapcsolati karakterlánccal beszerzett.</span><span class="sxs-lookup"><span data-stu-id="88cbd-125">Replace `[IoT hub connection string]` with the IoT hub connection string that you obtained.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="88cbd-126">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="88cbd-126">Read messages from your IoT hub</span></span>

<span data-ttu-id="88cbd-127">Ha egy TI SensorTag, ellenőrizze, hogy már kapcsolva a SensorTag.</span><span class="sxs-lookup"><span data-stu-id="88cbd-127">If you have a TI SensorTag, make sure you have already powered on your SensorTag.</span></span> <span data-ttu-id="88cbd-128">Futtassa az átjáró mintaalkalmazást, és olvashatja az IoT-központ által a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="88cbd-128">Run the gateway sample application and read IoT Hub messages by the following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="88cbd-129">A parancs a BLA mintaalkalmazás, amely beolvassa és csomagok a SensorTag vagy a szimulált eszköz hőmérséklete adatait, és az üzenet elküldése az IoT hub 2 másodpercenként futtatja.</span><span class="sxs-lookup"><span data-stu-id="88cbd-129">The command runs the BLE sample application that reads and packages temperature data from your SensorTag or simulated device and sends the message to your IoT hub every 2 seconds.</span></span> <span data-ttu-id="88cbd-130">Azt is indít egyik gyermekfolyamata az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="88cbd-130">It also spawns a child process to receive the message.</span></span>

<span data-ttu-id="88cbd-131">Által küldött és fogadott összes üzenetek azonnal jelenik meg az azonos konzolablak a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="88cbd-131">The messages that are being sent and received are all displayed instantly on the same console window in the host machine.</span></span> <span data-ttu-id="88cbd-132">A minta alkalmazáspéldány 40 másodperc múlva automatikusan leáll.</span><span class="sxs-lookup"><span data-stu-id="88cbd-132">The sample application instance will terminate automatically in 40 seconds.</span></span>

![BLA mintaalkalmazást a küldött és fogadott üzenetek](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a><span data-ttu-id="88cbd-134">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="88cbd-134">Summary</span></span>

<span data-ttu-id="88cbd-135">A mintakód üzenetek olvasásakor az IoT hub futtatását.</span><span class="sxs-lookup"><span data-stu-id="88cbd-135">You've run a sample code to read messages from your IoT hub.</span></span> <span data-ttu-id="88cbd-136">Készen áll az üzenetek az Azure table storage-ban tárolt olvasásához.</span><span class="sxs-lookup"><span data-stu-id="88cbd-136">You're ready to read the messages that are stored in your Azure table storage.</span></span>

## <a name="next-steps"></a><span data-ttu-id="88cbd-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88cbd-137">Next steps</span></span>
[<span data-ttu-id="88cbd-138">Azure-függvényalkalmazás és Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="88cbd-138">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


