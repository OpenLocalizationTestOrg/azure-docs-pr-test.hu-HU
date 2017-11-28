---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: üzeneteit |} Microsoft Docs"
description: "Futtassa a mintakódot az üzenetek olvasásakor az IoT hub a gazdaszámítógépen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok adatok"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5a6ec9c1-d83c-41c1-beaf-7c0d3395d77f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9fbf7958e2437d274f2692dbc235ac8147bdfa63
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="24095-104">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="24095-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="24095-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="24095-105">What you will do</span></span>

- <span data-ttu-id="24095-106">Futtassa a mintakódot az IoT hub üzenetek olvasásakor a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="24095-106">Run sample code on your host computer to read messages from your IoT hub.</span></span>

<span data-ttu-id="24095-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="24095-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="24095-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="24095-108">What you will learn</span></span>

<span data-ttu-id="24095-109">Hogyan használhatja a gulp eszközt az IoT hub üzeneteket beolvasni.</span><span class="sxs-lookup"><span data-stu-id="24095-109">How to use the gulp tool to read messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="24095-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="24095-110">What you need</span></span>

- <span data-ttu-id="24095-111">A szimulált eszköz minta [beállítása és futtatása a szimulált eszköz felhő feltöltése mintaalkalmazás](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="24095-111">The simulated device sample in [Configure and run a simulated device cloud upload sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="24095-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="24095-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="24095-113">Az eszköz kapcsolati karakterláncát a szimulált eszköz használják az IoT hub való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="24095-113">The device connection string is used by your simulated device to connect to your IoT hub.</span></span> <span data-ttu-id="24095-114">Az IoT hub kapcsolati karakterlánc biztosít az identitásjegyzékhez az IoT hub kezelheti az eszközöket, amelyek használata engedélyezett az IoT hub csatlakozni a csatlakozáshoz használt.</span><span class="sxs-lookup"><span data-stu-id="24095-114">The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub.</span></span>

- <span data-ttu-id="24095-115">Az erőforráscsoport az IoT hub listán a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="24095-115">List all your IoT hubs in your resource group by running the following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="24095-116">Használjon `iot-gateway` értékeként `{resource group name}` Ha nem módosítja azt.</span><span class="sxs-lookup"><span data-stu-id="24095-116">Use `iot-gateway` as the value of `{resource group name}` if you didn't change it.</span></span>
- <span data-ttu-id="24095-117">Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="24095-117">Get the IoT hub connection string by running the following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="24095-118">`{my hub name}`a rendszer az Ön által megadott nevét a 2.</span><span class="sxs-lookup"><span data-stu-id="24095-118">`{my hub name}` is the name that you specified in Lesson 2.</span></span>

## <a name="configure-the-device-connection-for-the-sample-code"></a><span data-ttu-id="24095-119">A mintakód az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24095-119">Configure the device connection for the sample code</span></span>

<span data-ttu-id="24095-120">Az IoT hub és az eszköz kapcsolat konfigurációja frissítése `config-azure.json` az alábbi lépések elvégzésével:</span><span class="sxs-lookup"><span data-stu-id="24095-120">Update IoT hub and device connection configurations in `config-azure.json` by performing the following steps:</span></span>

1. <span data-ttu-id="24095-121">Nyissa meg `config-azure.json` a Visual Studio Code a konzolablakban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="24095-121">Open `config-azure.json` in Visual Studio Code by running the following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="24095-122">Ellenőrizze a következő cserékhez a `config-azure.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="24095-122">Make the following replacements in the `config-azure.json` file:</span></span>

   ![az azure config képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="24095-124">Cserélje le `[IoT hub connection string]` az IoT hub kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="24095-124">Replace `[IoT hub connection string]` with the IoT hub connection string.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="24095-125">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="24095-125">Read messages from your IoT hub</span></span>

<span data-ttu-id="24095-126">Futtassa a szimulált eszköz mintaalkalmazást, és olvashatja az IoT-központ által a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="24095-126">Run the simulated device sample application and read IoT Hub messages by the following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="24095-127">A parancs futtatja az alkalmazást, amely üzeneteket küld az IoT hub 2 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="24095-127">The command runs the application that sends messages to your IoT hub every 2 seconds.</span></span> <span data-ttu-id="24095-128">Azt is indít egyik gyermekfolyamata az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="24095-128">It also spawns a child process to receive the message.</span></span>

<span data-ttu-id="24095-129">Által küldött és fogadott összes üzenetek azonnal jelenik meg az azonos konzolablak a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="24095-129">The messages that are being sent and received are all displayed instantly on the same console window in the host machine.</span></span> <span data-ttu-id="24095-130">Az alkalmazás 40 másodpercben kilép.</span><span class="sxs-lookup"><span data-stu-id="24095-130">The application will exit in 40 seconds.</span></span>

![Küldött és fogadott üzenetek szimulált mintaalkalmazás](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a><span data-ttu-id="24095-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="24095-132">Summary</span></span>

<span data-ttu-id="24095-133">Már sikeresen futott az IoT hub szimulált eszköz adatokat küldeni a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="24095-133">You've successfully run the sample application to send data to your IoT hub with simulated device.</span></span> <span data-ttu-id="24095-134">Az IoT hub elküldött üzenetek is olvasott.</span><span class="sxs-lookup"><span data-stu-id="24095-134">You've also read the messages that have been sent to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24095-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24095-135">Next steps</span></span>
[<span data-ttu-id="24095-136">Azure-függvényalkalmazás és Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="24095-136">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


