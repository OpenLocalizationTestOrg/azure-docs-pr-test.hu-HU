---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: üzeneteit |} Microsoft Docs"
description: "Futtassa a mintakód a fogadó számítógép tooread köszönőüzenetei az IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok hello felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok"
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
ms.openlocfilehash: 540575724bb5cdac4db581a226d8a02a59004d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="f415d-104">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="f415d-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="f415d-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="f415d-105">What you will do</span></span>

- <span data-ttu-id="f415d-106">Futtassa mintakód a gazdagépen futó számítógép tooread üzenetet az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f415d-106">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="f415d-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f415d-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f415d-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="f415d-108">What you will learn</span></span>

<span data-ttu-id="f415d-109">Hogyan toouse hello gulp az IoT hub eszköz tooread üzeneteit.</span><span class="sxs-lookup"><span data-stu-id="f415d-109">How toouse hello gulp tool tooread messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f415d-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="f415d-110">What you need</span></span>

- <span data-ttu-id="f415d-111">hello szimulált eszköz minta [beállítása és futtatása a szimulált eszköz felhő feltöltése mintaalkalmazás](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="f415d-111">hello simulated device sample in [Configure and run a simulated device cloud upload sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="f415d-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="f415d-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="f415d-113">a szimulált eszköz tooconnect tooyour IoT-központ hello eszköz kapcsolati karakterlánc használja.</span><span class="sxs-lookup"><span data-stu-id="f415d-113">hello device connection string is used by your simulated device tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="f415d-114">az IoT hub kapcsolati karakterlánc hello használt tooconnect toohello identitásjegyzékhez az IoT hub toomanage hello eszközök, amelyek számára engedélyezett tooconnect tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="f415d-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span>

- <span data-ttu-id="f415d-115">Az erőforráscsoport az IoT hub listában hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f415d-115">List all your IoT hubs in your resource group by running hello following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="f415d-116">Használjon `iot-gateway` hello értékeként `{resource group name}` Ha nem módosítja azt.</span><span class="sxs-lookup"><span data-stu-id="f415d-116">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change it.</span></span>
- <span data-ttu-id="f415d-117">Hello IoT hub kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f415d-117">Get hello IoT hub connection string by running hello following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="f415d-118">`{my hub name}`van hello Ön által megadott nevét a 2.</span><span class="sxs-lookup"><span data-stu-id="f415d-118">`{my hub name}` is hello name that you specified in Lesson 2.</span></span>

## <a name="configure-hello-device-connection-for-hello-sample-code"></a><span data-ttu-id="f415d-119">Hello mintakód hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f415d-119">Configure hello device connection for hello sample code</span></span>

<span data-ttu-id="f415d-120">Az IoT hub és az eszköz kapcsolat konfigurációja frissítése `config-azure.json` hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="f415d-120">Update IoT hub and device connection configurations in `config-azure.json` by performing hello following steps:</span></span>

1. <span data-ttu-id="f415d-121">Nyissa meg `config-azure.json` a Visual Studio Code hello a konzolablakban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f415d-121">Open `config-azure.json` in Visual Studio Code by running hello following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="f415d-122">Ellenőrizze a következő cserékhez a hello hello `config-azure.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="f415d-122">Make hello following replacements in hello `config-azure.json` file:</span></span>

   ![az azure config képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="f415d-124">Cserélje le `[IoT hub connection string]` a hello IoT hub kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="f415d-124">Replace `[IoT hub connection string]` with hello IoT hub connection string.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="f415d-125">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="f415d-125">Read messages from your IoT hub</span></span>

<span data-ttu-id="f415d-126">Hello szimulált eszköz mintaalkalmazás futtatása pedig beolvassák az IoT-központ üzenetek hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f415d-126">Run hello simulated device sample application and read IoT Hub messages by hello following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="f415d-127">hello parancs által küldött üzenetek tooyour IoT-központ 2 másodpercenként hello-alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="f415d-127">hello command runs hello application that sends messages tooyour IoT hub every 2 seconds.</span></span> <span data-ttu-id="f415d-128">Azt is egy gyermek folyamat tooreceive üdvözlőüzenetére indít.</span><span class="sxs-lookup"><span data-stu-id="f415d-128">It also spawns a child process tooreceive hello message.</span></span>

<span data-ttu-id="f415d-129">küldött és fogadott azonos konzolablakában az összes megjelenített azonnal hello köszönőüzenetei hello gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="f415d-129">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="f415d-130">hello alkalmazás 40 másodpercben kilép.</span><span class="sxs-lookup"><span data-stu-id="f415d-130">hello application will exit in 40 seconds.</span></span>

![Küldött és fogadott üzenetek szimulált mintaalkalmazás](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a><span data-ttu-id="f415d-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="f415d-132">Summary</span></span>

<span data-ttu-id="f415d-133">A szimulált eszköz sikeresen hello minta alkalmazás toosend adatok tooyour IoT-központ már futtatta.</span><span class="sxs-lookup"><span data-stu-id="f415d-133">You've successfully run hello sample application toosend data tooyour IoT hub with simulated device.</span></span> <span data-ttu-id="f415d-134">Az IoT-központ tooyour elküldött köszönőüzenetei is, hogy elolvasta.</span><span class="sxs-lookup"><span data-stu-id="f415d-134">You've also read hello messages that have been sent tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f415d-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f415d-135">Next steps</span></span>
[<span data-ttu-id="f415d-136">Azure-függvényalkalmazás és Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f415d-136">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


