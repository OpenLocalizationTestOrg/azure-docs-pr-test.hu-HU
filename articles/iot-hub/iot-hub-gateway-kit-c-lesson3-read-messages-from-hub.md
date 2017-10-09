---
title: "SensorTag eszköz & Azure IoT átjáró - rész 3: üzeneteit |} Microsoft Docs"
description: "Futtassa a mintakód a fogadó számítógép tooread köszönőüzenetei az IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok hello felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok"
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
ms.openlocfilehash: d3ffbe2e83f9d61c0088b8876a7f0eea62c1fbe1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="204e9-104">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="204e9-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="204e9-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="204e9-105">What you will do</span></span>

- <span data-ttu-id="204e9-106">Futtassa mintakód a gazdagépen futó számítógép tooread üzenetet az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="204e9-106">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="204e9-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="204e9-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="204e9-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="204e9-108">What you will learn</span></span>

<span data-ttu-id="204e9-109">Hogyan toouse hello gulp az IoT hub eszköz tooread üzeneteit.</span><span class="sxs-lookup"><span data-stu-id="204e9-109">How toouse hello gulp tool tooread messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="204e9-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="204e9-110">What you need</span></span>

- <span data-ttu-id="204e9-111">hello BLA mintaalkalmazás, amely a 3 sikeresen lefutott.</span><span class="sxs-lookup"><span data-stu-id="204e9-111">hello BLE sample application that you ran successfully in Lesson 3.</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="204e9-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="204e9-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="204e9-113">az eszköz (TI SensorTag vagy szimulált eszköz) tooconnect tooyour IoT hub hello eszköz kapcsolati karakterlánc használja.</span><span class="sxs-lookup"><span data-stu-id="204e9-113">hello device connection string is used by your device (TI SensorTag or simulated device) tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="204e9-114">az IoT hub kapcsolati karakterlánc hello használt tooconnect toohello identitásjegyzékhez az IoT hub toomanage hello eszközök, amelyek számára engedélyezett tooconnect tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="204e9-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span>

- <span data-ttu-id="204e9-115">Az erőforráscsoport az IoT hub listában hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="204e9-115">List all your IoT hubs in your resource group by running hello following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="204e9-116">Használjon `iot-gateway` hello értékeként `{resource group name}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="204e9-116">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value.</span></span>
- <span data-ttu-id="204e9-117">Hello IoT hub kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="204e9-117">Get hello IoT hub connection string by running hello following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="204e9-118">`{my hub name}`van hello Ön által megadott nevét a 2.</span><span class="sxs-lookup"><span data-stu-id="204e9-118">`{my hub name}` is hello name that you specified in Lesson 2.</span></span>

## <a name="configure-hello-device-connection-for-hello-sample-code"></a><span data-ttu-id="204e9-119">Hello mintakód hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="204e9-119">Configure hello device connection for hello sample code</span></span>

<span data-ttu-id="204e9-120">Frissítés hello eszköz konfigurációs fájl `config-azure.json` , hogy elolvashatja az IoT hub a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="204e9-120">Update hello device configuration file `config-azure.json` so that you can read messages from your IoT hub on your host computer.</span></span> <span data-ttu-id="204e9-121">toodo, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="204e9-121">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="204e9-122">Nyissa meg `config-azure.json` a Visual Studio Code hello a konzolablakban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="204e9-122">Open `config-azure.json` in Visual Studio Code by running hello following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="204e9-123">Ellenőrizze a következő cserékhez a hello hello `config-azure.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="204e9-123">Make hello following replacements in hello `config-azure.json` file:</span></span>

   ![az azure config képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="204e9-125">Cserélje le `[IoT hub connection string]` az IoT hub kapcsolati karakterlánc beszerzett hello.</span><span class="sxs-lookup"><span data-stu-id="204e9-125">Replace `[IoT hub connection string]` with hello IoT hub connection string that you obtained.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="204e9-126">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="204e9-126">Read messages from your IoT hub</span></span>

<span data-ttu-id="204e9-127">Ha egy TI SensorTag, ellenőrizze, hogy már kapcsolva a SensorTag.</span><span class="sxs-lookup"><span data-stu-id="204e9-127">If you have a TI SensorTag, make sure you have already powered on your SensorTag.</span></span> <span data-ttu-id="204e9-128">Hello átjáró mintaalkalmazás futtatása pedig beolvassák az IoT-központ üzenetek hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="204e9-128">Run hello gateway sample application and read IoT Hub messages by hello following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="204e9-129">hello parancs hello BLA mintaalkalmazás, amely beolvassa és a SensorTag vagy a szimulált eszköz hőmérséklete adatait a csomagokat és küldi hello üzenet tooyour IoT-központ 2 másodpercenként fut.</span><span class="sxs-lookup"><span data-stu-id="204e9-129">hello command runs hello BLE sample application that reads and packages temperature data from your SensorTag or simulated device and sends hello message tooyour IoT hub every 2 seconds.</span></span> <span data-ttu-id="204e9-130">Azt is egy gyermek folyamat tooreceive üdvözlőüzenetére indít.</span><span class="sxs-lookup"><span data-stu-id="204e9-130">It also spawns a child process tooreceive hello message.</span></span>

<span data-ttu-id="204e9-131">küldött és fogadott azonos konzolablakában az összes megjelenített azonnal hello köszönőüzenetei hello gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="204e9-131">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="204e9-132">hello minta alkalmazáspéldány 40 másodperc múlva automatikusan leáll.</span><span class="sxs-lookup"><span data-stu-id="204e9-132">hello sample application instance will terminate automatically in 40 seconds.</span></span>

![BLA mintaalkalmazást a küldött és fogadott üzenetek](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a><span data-ttu-id="204e9-134">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="204e9-134">Summary</span></span>

<span data-ttu-id="204e9-135">Az IoT hub a egy minta kód tooread üzenetek már futtatta.</span><span class="sxs-lookup"><span data-stu-id="204e9-135">You've run a sample code tooread messages from your IoT hub.</span></span> <span data-ttu-id="204e9-136">Ön az Azure table storage-ban tárolt készen tooread hello üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="204e9-136">You're ready tooread hello messages that are stored in your Azure table storage.</span></span>

## <a name="next-steps"></a><span data-ttu-id="204e9-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="204e9-137">Next steps</span></span>
[<span data-ttu-id="204e9-138">Azure-függvényalkalmazás és Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="204e9-138">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


