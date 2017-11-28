---
title: "Az Azure IoT-eszközök SDK C-hez |} Microsoft Docs"
description: "Ismerkedés az Azure IoT-eszközök SDK C-hez, és megtudhatja, hogyan hozzon létre egy IoT-központot kommunikáló eszközön futó alkalmazások."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 459b630f28fe48064f4ba280974f3fdbdb82f0a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-iot-device-sdk-for-c"></a><span data-ttu-id="8d964-103">Az Azure IoT-eszközök SDK C-hez</span><span class="sxs-lookup"><span data-stu-id="8d964-103">Azure IoT device SDK for C</span></span>

<span data-ttu-id="8d964-104">A **Azure IoT-eszközök SDK** úgy tervezték, hogy az üzenetek üzenetek küldése és fogadása egyszerűbbé szalagtárak készlete a **Azure IoT Hub** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8d964-104">The **Azure IoT device SDK** is a set of libraries designed to simplify the process of sending messages to and receiving messages from the **Azure IoT Hub** service.</span></span> <span data-ttu-id="8d964-105">Az SDK-t és egy adott platform célzó eltérő változata van, de ez a cikk ismerteti a **C-hez készült SDK Azure IoT-eszközök**.</span><span class="sxs-lookup"><span data-stu-id="8d964-105">There are different variations of the SDK, each targeting a specific platform, but this article describes the **Azure IoT device SDK for C**.</span></span>

<span data-ttu-id="8d964-106">Az Azure IoT-eszközök SDK c ANSI C maximalizálása hordozhatóságát (C99) nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="8d964-106">The Azure IoT device SDK for C is written in ANSI C (C99) to maximize portability.</span></span> <span data-ttu-id="8d964-107">A szolgáltatás elérhetővé teszi a könyvtárak az való működésre több platformokon és eszközökön, különösen akkor, ha a lemez minimalizálja a szolgáltatást, és memóriaigény prioritását.</span><span class="sxs-lookup"><span data-stu-id="8d964-107">This feature makes the libraries well-suited to operate on multiple platforms and devices, especially where minimizing disk and memory footprint is a priority.</span></span>

<span data-ttu-id="8d964-108">Számos platformon, amelyre az SDK-t tesztelték van (lásd: a [Azure IoT eszköz katalógus hitelesített](https://catalog.azureiotsuite.com/) részletekért).</span><span class="sxs-lookup"><span data-stu-id="8d964-108">There are a broad range of platforms on which the SDK has been tested (see the [Azure Certified for IoT device catalog](https://catalog.azureiotsuite.com/) for details).</span></span> <span data-ttu-id="8d964-109">Bár ez a cikk a Windows-platformon futó mintakód forgatókönyvek tartalmaz, akkor a jelen cikkben ismertetett kódot megegyezik a támogatott platformok tartományán keresztül.</span><span class="sxs-lookup"><span data-stu-id="8d964-109">Although this article includes walkthroughs of sample code running on the Windows platform, the code described in this article is identical across the range of supported platforms.</span></span>

<span data-ttu-id="8d964-110">Ez a cikk bemutatja az Azure IoT-eszközök SDK architektúrája a c kiszolgálóra. Azt mutatja be az eszköz kódtár inicializálása adatokat küldeni az IoT-központ és üzenetek fogadása.</span><span class="sxs-lookup"><span data-stu-id="8d964-110">This article introduces you to the architecture of the Azure IoT device SDK for C. It demonstrates how to initialize the device library, send data to IoT Hub, and receive messages from it.</span></span> <span data-ttu-id="8d964-111">A cikkben szereplő információkat kell lennie ahhoz, hogy az SDK használatának első, de a szalagtárakkal kapcsolatos további információkra mutató hivatkozások is biztosít.</span><span class="sxs-lookup"><span data-stu-id="8d964-111">The information in this article should be enough to get started using the SDK, but also provides pointers to additional information about the libraries.</span></span>

## <a name="sdk-architecture"></a><span data-ttu-id="8d964-112">SDK-architektúra</span><span class="sxs-lookup"><span data-stu-id="8d964-112">SDK architecture</span></span>

<span data-ttu-id="8d964-113">Megtalálhatja az [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) GitHub tárház és nézet adatai az API-nak a [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="8d964-113">You can find the [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of the API in the [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

<span data-ttu-id="8d964-114">A könyvtárak legújabb verziója található a **fő** a tárház főágába:</span><span class="sxs-lookup"><span data-stu-id="8d964-114">The latest version of the libraries can be found in the **master** branch of the repository:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* <span data-ttu-id="8d964-115">Az SDK alapvető megvalósítása szerepel a **IOT hubbal\_ügyfél** mappát, amely tartalmazza az SDK-ban a legalacsonyabb API réteg végrehajtásának: a **IoTHubClient** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8d964-115">The core implementation of the SDK is in the **iothub\_client** folder that contains the implementation of the lowest API layer in the SDK: the **IoTHubClient** library.</span></span> <span data-ttu-id="8d964-116">A **IoTHubClient** könyvtár végrehajtási nyers üzenetkezelési üzenetek küldése az IoT-központ és az üzenetek fogadása az IoT-központ API-kat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8d964-116">The **IoTHubClient** library contains APIs implementing raw messaging for sending messages to IoT Hub and receiving messages from IoT Hub.</span></span> <span data-ttu-id="8d964-117">Ha ezt a szalagtárat, üzenet szerializálási végrehajtásáért felelős, de egyéb adatainak kommunikál az IoT-központ történik meg.</span><span class="sxs-lookup"><span data-stu-id="8d964-117">When using this library, you are responsible for implementing message serialization, but other details of communicating with IoT Hub are handled for you.</span></span>
* <span data-ttu-id="8d964-118">A **szerializáló** mappa tartalmazza súgófunkciókat és példák bemutatják a előtt Azure IoT Hub küldése szerializálni az adatokat az ügyféloldali kódtár segítségével.</span><span class="sxs-lookup"><span data-stu-id="8d964-118">The **serializer** folder contains helper functions and samples that show you how to serialize data before sending to Azure IoT Hub using the client library.</span></span> <span data-ttu-id="8d964-119">A szerializáló használata nem kötelező, és biztosítja a könnyebb.</span><span class="sxs-lookup"><span data-stu-id="8d964-119">The use of the serializer is not mandatory and is provided as a convenience.</span></span> <span data-ttu-id="8d964-120">Használatához a **szerializáló** könyvtár, modell, amely meghatározza az IoT-központ és az üzenetek kap, várhatóan küldött adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="8d964-120">To use the **serializer** library, you define a model that specifies the data to send to IoT Hub and the messages you expect to receive from it.</span></span> <span data-ttu-id="8d964-121">Ha a modell van definiálva, az SDK-t biztosít, amelynek segítségével egyszerűen dolgozhat eszközről a felhőbe és felhő eszközre üzenetek anélkül, hogy szerializációs részleteinek API felületéhez.</span><span class="sxs-lookup"><span data-stu-id="8d964-121">Once the model is defined, the SDK provides you with an API surface that enables you to easily work with device-to-cloud and cloud-to-device messages without worrying about the serialization details.</span></span> <span data-ttu-id="8d964-122">A könyvtár más nyílt forráskódú tárak, amelyek megvalósítják az átviteli protokollal például MQTT és AMQP függ.</span><span class="sxs-lookup"><span data-stu-id="8d964-122">The library depends on other open source libraries that implement transport using protocols such as MQTT and AMQP.</span></span>
* <span data-ttu-id="8d964-123">A **IoTHubClient** könyvtár más nyílt forráskódú szalagtárak függ:</span><span class="sxs-lookup"><span data-stu-id="8d964-123">The **IoTHubClient** library depends on other open source libraries:</span></span>
  * <span data-ttu-id="8d964-124">A [Azure C megosztott segédprogram](https://github.com/Azure/azure-c-shared-utility) könyvtárban, amely több Azure-hoz kapcsolódó C SDK között szükséges alapvető feladatokhoz (például karakterláncok, lista manipuláció és IO) közös funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="8d964-124">The [Azure C shared utility](https://github.com/Azure/azure-c-shared-utility) library, which provides common functionality for basic tasks (such as strings, list manipulation, and IO) needed across several Azure-related C SDKs.</span></span>
  * <span data-ttu-id="8d964-125">A [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) könyvtárban, és azt egy AMQP korlátozott erőforrás eszközök optimalizálva ügyféloldali megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="8d964-125">The [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) library, which is a client-side implementation of AMQP optimized for resource constrained devices.</span></span>
  * <span data-ttu-id="8d964-126">A [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) könyvtárban, amely segíti a MQTT protokoll megvalósítását egy általános célú függvénytár, és korlátozott erőforrás eszközök optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="8d964-126">The [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) library, which is a general-purpose library implementing the MQTT protocol and optimized for resource constrained devices.</span></span>

<span data-ttu-id="8d964-127">Ezek a könyvtárak használata kódpéldákat megnézzük megérteni.</span><span class="sxs-lookup"><span data-stu-id="8d964-127">Use of these libraries is easier to understand by looking at example code.</span></span> <span data-ttu-id="8d964-128">A következő szakaszok végigvezetik Önt az alkalmazásokra, amelyek szerepelnek az SDK számos.</span><span class="sxs-lookup"><span data-stu-id="8d964-128">The following sections walk you through several of the sample applications that are included in the SDK.</span></span> <span data-ttu-id="8d964-129">Ez a forgatókönyv biztosítani fogja a helyes működését a különböző képességeinek a architekturális rétegek az SDK és az API-k működése bemutatása.</span><span class="sxs-lookup"><span data-stu-id="8d964-129">This walkthrough should give you a good feel for the various capabilities of the architectural layers of the SDK and an introduction to how the APIs work.</span></span>

## <a name="before-you-run-the-samples"></a><span data-ttu-id="8d964-130">A minták futtatása előtt</span><span class="sxs-lookup"><span data-stu-id="8d964-130">Before you run the samples</span></span>

<span data-ttu-id="8d964-131">A minták c futtathatja az Azure IoT-eszközök SDK, előtt [hozzon létre egy példányt az IoT-központ szolgáltatás](iot-hub-create-through-portal.md) az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="8d964-131">Before you can run the samples in the Azure IoT device SDK for C, you must [create an instance of the IoT Hub service](iot-hub-create-through-portal.md) in your Azure subscription.</span></span> <span data-ttu-id="8d964-132">Fejezze be a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="8d964-132">Then complete the following tasks:</span></span>

* <span data-ttu-id="8d964-133">A fejlesztőkörnyezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="8d964-133">Prepare your development environment</span></span>
* <span data-ttu-id="8d964-134">Szerezze be a hitelesítő adatai.</span><span class="sxs-lookup"><span data-stu-id="8d964-134">Obtain device credentials.</span></span>

### <a name="prepare-your-development-environment"></a><span data-ttu-id="8d964-135">A fejlesztőkörnyezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="8d964-135">Prepare your development environment</span></span>

<span data-ttu-id="8d964-136">Csomagok (például NuGet Windows vagy apt_get Debian és Ubuntu) közös platformok kapnak, és a minták használja ezeket a csomagokat, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="8d964-136">Packages are provided for common platforms (such as NuGet for Windows or apt_get for Debian and Ubuntu) and the samples use these packages when available.</span></span> <span data-ttu-id="8d964-137">Bizonyos esetekben kell lefordítani az SDK-t a vagy az eszközön.</span><span class="sxs-lookup"><span data-stu-id="8d964-137">In some cases, you need to compile the SDK for or on your device.</span></span> <span data-ttu-id="8d964-138">Ha az SDK fordítási szüksége, tekintse meg [a fejlesztőkörnyezet előkészítése](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="8d964-138">If you need to compile the SDK, see [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) in the GitHub repository.</span></span>

<span data-ttu-id="8d964-139">A minta alkalmazáskód beszerzéséhez töltse le az SDK-t a Githubról.</span><span class="sxs-lookup"><span data-stu-id="8d964-139">To obtain the sample application code, download a copy of the SDK from GitHub.</span></span> <span data-ttu-id="8d964-140">A forrás a példányt a **fő** ága a [GitHub-tárházban](https://github.com/Azure/azure-iot-sdk-c).</span><span class="sxs-lookup"><span data-stu-id="8d964-140">Get your copy of the source from the **master** branch of the [GitHub repository](https://github.com/Azure/azure-iot-sdk-c).</span></span>


### <a name="obtain-the-device-credentials"></a><span data-ttu-id="8d964-141">Szerezze be a hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="8d964-141">Obtain the device credentials</span></span>

<span data-ttu-id="8d964-142">Most, hogy a forrás mintakód, a következő lépés az eszköz hitelesítő adatokat lekérni.</span><span class="sxs-lookup"><span data-stu-id="8d964-142">Now that you have the sample source code, the next thing to do is to get a set of device credentials.</span></span> <span data-ttu-id="8d964-143">Az eszközök hozzáférhetnek az IoT-központ először hozzá kell az eszközt biztosít az IoT-központ identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="8d964-143">For a device to be able to access an IoT hub, you must first add the device to the IoT Hub identity registry.</span></span> <span data-ttu-id="8d964-144">Az eszköz hozzáadásakor le, amelyekre szüksége van az eszköz kell kapcsolódnia kell az IoT hub eszköz hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="8d964-144">When you add your device, you get a set of device credentials that you need for the device to be able to connect to the IoT hub.</span></span> <span data-ttu-id="8d964-145">A következő szakaszban tárgyalt mintaalkalmazások ezeket a hitelesítő adatokat várt formájában egy **eszköz kapcsolati karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="8d964-145">The sample applications discussed in the next section expect these credentials in the form of a **device connection string**.</span></span>

<span data-ttu-id="8d964-146">Számos nyílt forráskódú eszköz kezeléséhez az IoT hub van.</span><span class="sxs-lookup"><span data-stu-id="8d964-146">There are several open source tools to help you manage your IoT hub.</span></span>

* <span data-ttu-id="8d964-147">Egy nevű Windows-alkalmazás [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="8d964-147">A Windows application called [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>
* <span data-ttu-id="8d964-148">A platformok közötti node.js parancssori eszköz neve [IOT hubbal-explorer](https://github.com/azure/iothub-explorer).</span><span class="sxs-lookup"><span data-stu-id="8d964-148">A cross-platform node.js CLI tool called [iothub-explorer](https://github.com/azure/iothub-explorer).</span></span>

<span data-ttu-id="8d964-149">Ez az oktatóanyag használja a grafikus *eszköz explorer* eszköz.</span><span class="sxs-lookup"><span data-stu-id="8d964-149">This tutorial uses the graphical *device explorer* tool.</span></span> <span data-ttu-id="8d964-150">Használhatja a *IOT hubbal-explorer* eszköze, ha szeretné használni a parancssori eszköz.</span><span class="sxs-lookup"><span data-stu-id="8d964-150">You can also use the *iothub-explorer* tool if you prefer to use a CLI tool.</span></span>

<span data-ttu-id="8d964-151">Az eszköz explorer eszközt az Azure IoT szolgáltatás-tárakat használ az IoT-központ, beleértve az eszközök különféle funkciók elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="8d964-151">The device explorer tool uses the Azure IoT service libraries to perform various functions on IoT Hub, including adding devices.</span></span> <span data-ttu-id="8d964-152">Ha az eszköz explorer eszköz segítségével hozzáad egy eszközt, az eszközhöz kapott egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8d964-152">If you use the device explorer tool to add a device, you get a connection string for your device.</span></span> <span data-ttu-id="8d964-153">Ez a kapcsolati karakterlánc a minta-alkalmazások futtatására van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8d964-153">You need this connection string to run the sample applications.</span></span>

<span data-ttu-id="8d964-154">Ha nem ismeri az eszköz explorer eszközzel, az alábbi eljárás ismerteti, hogyan használhatja hozzáad egy eszközt, és szerezze be az eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8d964-154">If you're not familiar with the device explorer tool, the following procedure describes how to use it to add a device and obtain a device connection string.</span></span>

<span data-ttu-id="8d964-155">Az eszköz explorer eszköz telepítéséhez tekintse át [az IoT Hub-eszközöknek eszköz kezelővel](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="8d964-155">To install the device explorer tool, see [How to use the Device Explorer for IoT Hub devices](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>

<span data-ttu-id="8d964-156">Amikor futtatja a programot, ez az interfész jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8d964-156">When you run the program, you see this interface:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

<span data-ttu-id="8d964-157">Adja meg a **IoT Hub kapcsolati karakterlánc** a első mezőben, majd kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="8d964-157">Enter your **IoT Hub Connection String** in the first field and click **Update**.</span></span> <span data-ttu-id="8d964-158">Ebben a lépésben konfigurálja az eszközt, hogy kommunikáljon az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="8d964-158">This step configures the tool so that it can communicate with IoT Hub.</span></span>

<span data-ttu-id="8d964-159">Az IoT-központ kapcsolati karakterlánc konfigurálását, kattintson a **felügyeleti** lapon:</span><span class="sxs-lookup"><span data-stu-id="8d964-159">When the IoT Hub connection string is configured, click the **Management** tab:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

<span data-ttu-id="8d964-160">Ez a lap majdnem, amelyeken kezelheti az IoT hub-ben regisztrált eszközök.</span><span class="sxs-lookup"><span data-stu-id="8d964-160">This tab is where you manage the devices registered in your IoT hub.</span></span>

<span data-ttu-id="8d964-161">Egy eszköz kattintva hozhat létre a **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="8d964-161">You create a device by clicking the **Create** button.</span></span> <span data-ttu-id="8d964-162">Olyan előre megadott kulcsokat (elsődleges és másodlagos) egy párbeszédpanelt jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="8d964-162">A dialog displays with a set of pre-populated keys (primary and secondary).</span></span> <span data-ttu-id="8d964-163">Adjon meg egy **Eszközazonosító** majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8d964-163">Enter a **Device ID** and then click **Create**.</span></span>

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

<span data-ttu-id="8d964-164">Amikor az eszköz jön létre, az eszközök frissítések a regisztrált eszközökkel, beleértve a most létrehozott egy listán.</span><span class="sxs-lookup"><span data-stu-id="8d964-164">When the device is created, the Devices list updates with all the registered devices, including the one you just created.</span></span> <span data-ttu-id="8d964-165">Az új eszköz a jobb gombbal, az ebben a menüben jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8d964-165">If you right-click your new device, you see this menu:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

<span data-ttu-id="8d964-166">Ha úgy dönt, **másolja a kijelölt eszközre vonatkozóan a kapcsolati karakterlánc**, az eszköz kapcsolati karakterláncot a vágólapra másolódik.</span><span class="sxs-lookup"><span data-stu-id="8d964-166">If you choose **Copy connection string for selected device**, the device connection string is copied to the clipboard.</span></span> <span data-ttu-id="8d964-167">Tartsa meg az eszköz kapcsolati karakterlánc másolatát.</span><span class="sxs-lookup"><span data-stu-id="8d964-167">Keep a copy of the device connection string.</span></span> <span data-ttu-id="8d964-168">Meg kell az alábbi szakaszokban ismertetett mintaalkalmazások futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="8d964-168">You need it when running the sample applications described in the following sections.</span></span>

<span data-ttu-id="8d964-169">A fenti lépések végrehajtását, készen áll arra, hogy elindítsa néhány kódot.</span><span class="sxs-lookup"><span data-stu-id="8d964-169">When you've completed the steps above, you're ready to start running some code.</span></span> <span data-ttu-id="8d964-170">Mindkét minták állandó rendelkezik, amely lehetővé teszi, hogy adjon meg egy kapcsolati karakterláncot fő forrásfájl tetején.</span><span class="sxs-lookup"><span data-stu-id="8d964-170">Both samples have a constant at the top of the main source file that enables you to enter a connection string.</span></span> <span data-ttu-id="8d964-171">Például a megfelelő sor a **IOT hubbal\_ügyfél\_minta\_mqtt** alkalmazás a következőképpen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8d964-171">For example, the corresponding line from the **iothub\_client\_sample\_mqtt** application appears as follows.</span></span>

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-the-iothubclient-library"></a><span data-ttu-id="8d964-172">A IoTHubClient könyvtárban</span><span class="sxs-lookup"><span data-stu-id="8d964-172">Use the IoTHubClient library</span></span>

<span data-ttu-id="8d964-173">Belül a **IOT hubbal\_ügyfél** mappájában a [azure iot-sdk--c](https://github.com/azure/azure-iot-sdk-c) -tárházban, van egy **minták** nevű mappát, amely tartalmazza az alkalmazás **IOT hubbal\_ügyfél\_minta\_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="8d964-173">Within the **iothub\_client** folder in the [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) repository, there is a **samples** folder that contains an application called **iothub\_client\_sample\_mqtt**.</span></span>

<span data-ttu-id="8d964-174">A Windows-verzión a **IOT hubbal\_ügyfél\_minta\_mqtt** alkalmazás tartalmazza a következő Visual Studio megoldás:</span><span class="sxs-lookup"><span data-stu-id="8d964-174">The Windows version of the **iothub\_client\_sample\_mqtt** application includes the following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="8d964-175">Ha megnyitja a projektet a Visual Studio 2017, fogadja el az utasításokat a legújabb verzióra a projekt átirányítása.</span><span class="sxs-lookup"><span data-stu-id="8d964-175">If you open this project in Visual Studio 2017, accept the prompts to retarget the project to the latest version.</span></span>

<span data-ttu-id="8d964-176">Ez a megoldás egyetlen projektet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8d964-176">This solution contains a single project.</span></span> <span data-ttu-id="8d964-177">Nincsenek telepítve az ebben a megoldásban négy NuGet-csomagok:</span><span class="sxs-lookup"><span data-stu-id="8d964-177">There are four NuGet packages installed in this solution:</span></span>

* <span data-ttu-id="8d964-178">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="8d964-178">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="8d964-179">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="8d964-179">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="8d964-180">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="8d964-180">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="8d964-181">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="8d964-181">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="8d964-182">Mindig van szüksége a **Microsoft.Azure.C.SharedUtility** csomag, amikor az SDK kezel.</span><span class="sxs-lookup"><span data-stu-id="8d964-182">You always need the **Microsoft.Azure.C.SharedUtility** package when you are working with the SDK.</span></span> <span data-ttu-id="8d964-183">Ez a minta a MQTT protokollt használja, ezért meg kell adni a **Microsoft.Azure.umqtt** és **Microsoft.Azure.IoTHub.MqttTransport** csomagok (van egyenértékű csomagjai amqp-t és HTTP).</span><span class="sxs-lookup"><span data-stu-id="8d964-183">This sample uses the MQTT protocol, therefore you must include the **Microsoft.Azure.umqtt** and **Microsoft.Azure.IoTHub.MqttTransport** packages (there are equivalent packages for AMQP and HTTP).</span></span> <span data-ttu-id="8d964-184">Mivel a mintát használ a **IoTHubClient** könyvtár, fel kell venni a **Microsoft.Azure.IoTHub.IoTHubClient** csomag a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="8d964-184">Because the sample uses the **IoTHubClient** library, you must also include the **Microsoft.Azure.IoTHub.IoTHubClient** package in your solution.</span></span>

<span data-ttu-id="8d964-185">A mintaalkalmazáshoz végrehajtására is megtalálhatja a **IOT hubbal\_ügyfél\_minta\_mqtt.c** forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="8d964-185">You can find the implementation for the sample application in the **iothub\_client\_sample\_mqtt.c** source file.</span></span>

<span data-ttu-id="8d964-186">Az alábbi lépéseket a mintaalkalmazás segítségével végigvezetik mi van kell használniuk a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8d964-186">The following steps use this sample application to walk you through what's required to use the **IoTHubClient** library.</span></span>

### <a name="initialize-the-library"></a><span data-ttu-id="8d964-187">A kódtár inicializálása</span><span class="sxs-lookup"><span data-stu-id="8d964-187">Initialize the library</span></span>

> [!NOTE]
> <span data-ttu-id="8d964-188">A könyvtárakkal használatának megkezdése, előtt szükség lehet néhány platform-specifikus inicializálási végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="8d964-188">Before you start working with the libraries, you may need to perform some platform-specific initialization.</span></span> <span data-ttu-id="8d964-189">Például ha AMQP használatához Linux inicializálja a OpenSSL-könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8d964-189">For example, if you plan to use AMQP on Linux you must initialize the OpenSSL library.</span></span> <span data-ttu-id="8d964-190">A minták a [GitHub-tárházban](https://github.com/Azure/azure-iot-sdk-c) segédprogram függvény **platform\_init** mikor az ügyfél elindul, és hívja meg a **platform\_deinit** függvény való kilépés előtt.</span><span class="sxs-lookup"><span data-stu-id="8d964-190">The samples in the [GitHub repository](https://github.com/Azure/azure-iot-sdk-c) call the utility function **platform\_init** when the client starts and call the **platform\_deinit** function before exiting.</span></span> <span data-ttu-id="8d964-191">Ezek a funkciók platform.h fejlécfájlt van deklarálva.</span><span class="sxs-lookup"><span data-stu-id="8d964-191">These functions are declared in the platform.h header file.</span></span> <span data-ttu-id="8d964-192">Ezeket a funkciókat a célplatformot a definíciók vizsgálja meg a [tárház](https://github.com/Azure/azure-iot-sdk-c) annak meghatározásához, hogy szükséges-e a platform-specifikus inicializálási kód szerepeljenek az ügyfél.</span><span class="sxs-lookup"><span data-stu-id="8d964-192">Examine the definitions of these functions for your target platform in the [repository](https://github.com/Azure/azure-iot-sdk-c) to determine whether you need to include any platform-specific initialization code in your client.</span></span>

<span data-ttu-id="8d964-193">A könyvtárakkal elindításához foglaljon le egy IoT-központ ügyfél leíró:</span><span class="sxs-lookup"><span data-stu-id="8d964-193">To start working with the libraries, first allocate an IoT Hub client handle:</span></span>

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

<span data-ttu-id="8d964-194">Adja meg az eszköz kapcsolati karakterlánc szerezte be az eszköz explorer eszközből a függvény egy példányát.</span><span class="sxs-lookup"><span data-stu-id="8d964-194">You pass a copy of the device connection string you obtained from the device explorer tool to this function.</span></span> <span data-ttu-id="8d964-195">Is megjelölhetők használt kommunikációs protokollt.</span><span class="sxs-lookup"><span data-stu-id="8d964-195">You also designate the communications protocol to use.</span></span> <span data-ttu-id="8d964-196">Ez a példa MQTT, de amqp-t és HTTP is lehetősége.</span><span class="sxs-lookup"><span data-stu-id="8d964-196">This example uses MQTT, but AMQP and HTTP are also options.</span></span>

<span data-ttu-id="8d964-197">Ha rendelkezik olyan érvényes **IOT HUBBAL\_ügyfél\_KEZELNI**, megkezdheti a hívja az API-k és az IoT-központ üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="8d964-197">When you have a valid **IOTHUB\_CLIENT\_HANDLE**, you can start calling the APIs to send and receive messages to and from IoT Hub.</span></span>

### <a name="send-messages"></a><span data-ttu-id="8d964-198">Üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="8d964-198">Send messages</span></span>

<span data-ttu-id="8d964-199">A mintaalkalmazás üzeneteket küldhet az IoT hub beállítja a hurok.</span><span class="sxs-lookup"><span data-stu-id="8d964-199">The sample application sets up a loop to send messages to your IoT hub.</span></span> <span data-ttu-id="8d964-200">A következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="8d964-200">The following snippet:</span></span>

- <span data-ttu-id="8d964-201">Létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="8d964-201">Creates a message.</span></span>
- <span data-ttu-id="8d964-202">Tulajdonság hozzáadása az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="8d964-202">Adds a property to the message.</span></span>
- <span data-ttu-id="8d964-203">Üzenet küldése.</span><span class="sxs-lookup"><span data-stu-id="8d964-203">Sends a message.</span></span>

<span data-ttu-id="8d964-204">Először hozzon létre egy üzenetet:</span><span class="sxs-lookup"><span data-stu-id="8d964-204">First, create a message:</span></span>

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission to IoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

<span data-ttu-id="8d964-205">Minden alkalommal, amikor egy üzenetet küld, meg kell adnia egy hivatkozást egy visszahívási függvény, amelyet az adatok elküldése során.</span><span class="sxs-lookup"><span data-stu-id="8d964-205">Every time you send a message, you specify a reference to a callback function that's invoked when the data is sent.</span></span> <span data-ttu-id="8d964-206">Ebben a példában a visszahívási függvény hívása esetén **SendConfirmationCallback**.</span><span class="sxs-lookup"><span data-stu-id="8d964-206">In this example, the callback function is called **SendConfirmationCallback**.</span></span> <span data-ttu-id="8d964-207">Az alábbi kódrészletben láthatja a visszahívási függvény:</span><span class="sxs-lookup"><span data-stu-id="8d964-207">The following snippet shows this callback function:</span></span>

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

<span data-ttu-id="8d964-208">Vegye figyelembe a hívást a **IoTHubMessage\_Destroy** működik, ha az üzenettel elkészült.</span><span class="sxs-lookup"><span data-stu-id="8d964-208">Note the call to the **IoTHubMessage\_Destroy** function when you're done with the message.</span></span> <span data-ttu-id="8d964-209">Ez a funkció ennyi helyet szabadít a feladatoknak az üzenet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="8d964-209">This function frees the resources allocated when you created the message.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="8d964-210">Hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="8d964-210">Receive messages</span></span>

<span data-ttu-id="8d964-211">Egy üzenet fogadását egy aszinkron művelet.</span><span class="sxs-lookup"><span data-stu-id="8d964-211">Receiving a message is an asynchronous operation.</span></span> <span data-ttu-id="8d964-212">Először regisztrálnia a visszahívás meghívni, amikor az eszköz egy üzenetet kapja:</span><span class="sxs-lookup"><span data-stu-id="8d964-212">First, you register the callback to invoke when the device receives a message:</span></span>

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

<span data-ttu-id="8d964-213">Az utolsó paramétere "void" mutató függetlenül szeretné.</span><span class="sxs-lookup"><span data-stu-id="8d964-213">The last parameter is a void pointer to whatever you want.</span></span> <span data-ttu-id="8d964-214">A minta egész mutató, de akkor lehet, hogy olyan összetettebb adatszerkezet mutató hivatkozások.</span><span class="sxs-lookup"><span data-stu-id="8d964-214">In the sample, it's a pointer to an integer but it could be a pointer to a more complex data structure.</span></span> <span data-ttu-id="8d964-215">Ez a paraméter lehetővé teszi, hogy működik a funkció a hívó rendelkező megosztott állapot visszahívási függvényt.</span><span class="sxs-lookup"><span data-stu-id="8d964-215">This parameter enables the callback function to operate on shared state with the caller of this function.</span></span>

<span data-ttu-id="8d964-216">Amikor az eszköz üzenetet kap, a regisztrált visszahívási függvényt hívják.</span><span class="sxs-lookup"><span data-stu-id="8d964-216">When the device receives a message, the registered callback function is invoked.</span></span> <span data-ttu-id="8d964-217">A visszahívási függvény kéri le:</span><span class="sxs-lookup"><span data-stu-id="8d964-217">This callback function retrieves:</span></span>

* <span data-ttu-id="8d964-218">Üzenet azonosítója és az üzenet korrelációs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8d964-218">The message id and correlation id from the message.</span></span>
* <span data-ttu-id="8d964-219">Az üzenet tartalmát.</span><span class="sxs-lookup"><span data-stu-id="8d964-219">The message content.</span></span>
* <span data-ttu-id="8d964-220">Az üzenet egyéni tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="8d964-220">Any custom properties from the message.</span></span>

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable to retrieve the message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive the work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from the message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="8d964-221">Használja a **IoTHubMessage\_GetByteArray** működnek, mint az üzenet, amely ebben a példában a karakterlánc beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8d964-221">Use the **IoTHubMessage\_GetByteArray** function to retrieve the message, which in this example is a string.</span></span>

### <a name="uninitialize-the-library"></a><span data-ttu-id="8d964-222">A könyvtár meghívná</span><span class="sxs-lookup"><span data-stu-id="8d964-222">Uninitialize the library</span></span>

<span data-ttu-id="8d964-223">Amikor elkészült, eseményeket küldő és fogadó üzenetek, akkor is meghívná az IoT-könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8d964-223">When you're done sending events and receiving messages, you can uninitialize the IoT library.</span></span> <span data-ttu-id="8d964-224">Ehhez adja ki a következő függvény hívásához szükséges:</span><span class="sxs-lookup"><span data-stu-id="8d964-224">To do so, issue the following function call:</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="8d964-225">Ez a hívás területet szabadít fel az erőforrások korábban le van foglalva a **IoTHubClient\_CreateFromConnectionString** függvény.</span><span class="sxs-lookup"><span data-stu-id="8d964-225">This call frees up the resources previously allocated by the **IoTHubClient\_CreateFromConnectionString** function.</span></span>

<span data-ttu-id="8d964-226">Ahogy látja, nem az üzeneteket küldjön és fogadjon a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8d964-226">As you can see, it's easy to send and receive messages with the **IoTHubClient** library.</span></span> <span data-ttu-id="8d964-227">A könyvtár kommunikál az IoT-központot, beleértve a használandó protokoll részleteit kezeli (a fejlesztői szempontjából, ez a lehetőség egy egyszerű konfiguráció).</span><span class="sxs-lookup"><span data-stu-id="8d964-227">The library handles the details of communicating with IoT Hub, including which protocol to use (from the perspective of the developer, this is a simple configuration option).</span></span>

<span data-ttu-id="8d964-228">A **IoTHubClient** kódtár is biztosít a pontos szabályozhatják, hogyan kell szerializálni az adatokat az IoT-központ küld az eszköz.</span><span class="sxs-lookup"><span data-stu-id="8d964-228">The **IoTHubClient** library also provides precise control over how to serialize the data your device sends to IoT Hub.</span></span> <span data-ttu-id="8d964-229">Egyes esetekben ez az érték előnyös, de más nem kívánt az érintett megvalósításához tartozó részlet.</span><span class="sxs-lookup"><span data-stu-id="8d964-229">In some cases this level of control is an advantage, but in others it is an implementation detail that you don't want to be concerned with.</span></span> <span data-ttu-id="8d964-230">Ha ez a helyzet, akkor érdemes használatával a **szerializáló** függvénytár, amely a következő szakaszban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="8d964-230">If that's the case, you might consider using the **serializer** library, which is described in the next section.</span></span>

## <a name="use-the-serializer-library"></a><span data-ttu-id="8d964-231">A szerializáló könyvtárban</span><span class="sxs-lookup"><span data-stu-id="8d964-231">Use the serializer library</span></span>

<span data-ttu-id="8d964-232">Hasonlóak a **szerializáló** könyvtár helyezkedik el, a a **IoTHubClient** könyvtár az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="8d964-232">Conceptually the **serializer** library sits on top of the **IoTHubClient** library in the SDK.</span></span> <span data-ttu-id="8d964-233">Használja a **IoTHubClient** könyvtár az IoT-központ, de a mögöttes kommunikáció hozzáadja az terheket üzenet szerializálási való eltávolításához a fejlesztőtől származó modellezési képességekkel.</span><span class="sxs-lookup"><span data-stu-id="8d964-233">It uses the **IoTHubClient** library for the underlying communication with IoT Hub, but it adds modeling capabilities that remove the burden of dealing with message serialization from the developer.</span></span> <span data-ttu-id="8d964-234">Hogyan a szalagtár works legjobb bemutatott példa szerint.</span><span class="sxs-lookup"><span data-stu-id="8d964-234">How this library works is best demonstrated by an example.</span></span>

<span data-ttu-id="8d964-235">Belül a **szerializáló** mappájában a [azure iot-sdk--c tárház](https://github.com/Azure/azure-iot-sdk-c), van egy **minták** nevű mappát, amely tartalmazza az alkalmazás **simplesample\_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="8d964-235">Inside the **serializer** folder in the [azure-iot-sdk-c repository](https://github.com/Azure/azure-iot-sdk-c), is a **samples** folder that contains an application called **simplesample\_mqtt**.</span></span> <span data-ttu-id="8d964-236">Ez a minta Windows verziója a következő Visual Studio megoldás tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8d964-236">The Windows version of this sample includes the following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="8d964-237">Ha megnyitja a projektet a Visual Studio 2017, fogadja el az utasításokat a legújabb verzióra a projekt átirányítása.</span><span class="sxs-lookup"><span data-stu-id="8d964-237">If you open this project in Visual Studio 2017, accept the prompts to retarget the project to the latest version.</span></span>

<span data-ttu-id="8d964-238">A fenti példában az a több NuGet-csomagok foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="8d964-238">As with the previous sample, this one includes several NuGet packages:</span></span>

* <span data-ttu-id="8d964-239">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="8d964-239">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="8d964-240">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="8d964-240">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="8d964-241">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="8d964-241">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="8d964-242">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="8d964-242">Microsoft.Azure.IoTHub.Serializer</span></span>
* <span data-ttu-id="8d964-243">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="8d964-243">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="8d964-244">Most láthatta, ezeket a csomagokat az előző minta a legtöbb, de **Microsoft.Azure.IoTHub.Serializer** új.</span><span class="sxs-lookup"><span data-stu-id="8d964-244">You've seen most of these packages in the previous sample, but **Microsoft.Azure.IoTHub.Serializer** is new.</span></span> <span data-ttu-id="8d964-245">Ez a csomag szükséges használata esetén a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8d964-245">This package is required when you use the **serializer** library.</span></span>

<span data-ttu-id="8d964-246">A mintaalkalmazáshoz végrehajtásának megtalálhatja a **simplesample\_mqtt.c** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8d964-246">You can find the implementation of the sample application in the **simplesample\_mqtt.c** file.</span></span>

<span data-ttu-id="8d964-247">A következő szakaszok ismerteti a minta kulcs részei között.</span><span class="sxs-lookup"><span data-stu-id="8d964-247">The following sections walk you through the key parts of this sample.</span></span>

### <a name="initialize-the-library"></a><span data-ttu-id="8d964-248">A kódtár inicializálása</span><span class="sxs-lookup"><span data-stu-id="8d964-248">Initialize the library</span></span>

<span data-ttu-id="8d964-249">A munka megkezdéséhez a **szerializáló** könyvtár, hívja az API-k inicializálása:</span><span class="sxs-lookup"><span data-stu-id="8d964-249">To start working with the **serializer** library, call the initialization APIs:</span></span>

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

<span data-ttu-id="8d964-250">A hívás a **szerializáló\_init** függvény egyszeri hívás és inicializálja az alapul szolgáló könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8d964-250">The call to the **serializer\_init** function is a one-time call and initializes the underlying library.</span></span> <span data-ttu-id="8d964-251">Ezután meghívja a **IoTHubClient\_inden\_CreateFromConnectionString** -függvény, mint az azonos API a **IoTHubClient** minta.</span><span class="sxs-lookup"><span data-stu-id="8d964-251">Then, you call the **IoTHubClient\_LL\_CreateFromConnectionString** function, which is the same API as in the **IoTHubClient** sample.</span></span> <span data-ttu-id="8d964-252">Ez a hívás beállítja az eszköz kapcsolati karakterlánc (Ez a hívás is, ha úgy dönt, hogy a protokoll használni kívánt).</span><span class="sxs-lookup"><span data-stu-id="8d964-252">This call sets your device connection string (this call is also where you choose the protocol you want to use).</span></span> <span data-ttu-id="8d964-253">Ez a minta MQTT használja, mint az átvitel, de AMQP vagy HTTP használhatja.</span><span class="sxs-lookup"><span data-stu-id="8d964-253">This sample uses MQTT as the transport, but could use AMQP or HTTP.</span></span>

<span data-ttu-id="8d964-254">Végezetül hívja a **létrehozása\_MODELL\_példány** függvény.</span><span class="sxs-lookup"><span data-stu-id="8d964-254">Finally, call the **CREATE\_MODEL\_INSTANCE** function.</span></span> <span data-ttu-id="8d964-255">**WeatherStation** a modell névtere és **ContosoAnemometer** a modell neve.</span><span class="sxs-lookup"><span data-stu-id="8d964-255">**WeatherStation** is the namespace of the model and **ContosoAnemometer** is the name of the model.</span></span> <span data-ttu-id="8d964-256">A modell-példány létrehozása után üzenetek küldése és fogadása indításához használhatja.</span><span class="sxs-lookup"><span data-stu-id="8d964-256">Once the model instance is created, you can use it to start sending and receiving messages.</span></span> <span data-ttu-id="8d964-257">Fontos azonban megérteni milyen modell.</span><span class="sxs-lookup"><span data-stu-id="8d964-257">However, it's important to understand what a model is.</span></span>

### <a name="define-the-model"></a><span data-ttu-id="8d964-258">A modell meghatározása</span><span class="sxs-lookup"><span data-stu-id="8d964-258">Define the model</span></span>

<span data-ttu-id="8d964-259">Egy modell a(z) a **szerializáló** könyvtár határozza meg, az üzenetek, amely az eszköz küldhet az IoT-központ és az üzenetek, az úgynevezett *műveletek* megkaphatja a modellezési nyelven.</span><span class="sxs-lookup"><span data-stu-id="8d964-259">A model in the **serializer** library defines the messages that your device can send to IoT Hub and the messages, called *actions* in the modeling language, which it can receive.</span></span> <span data-ttu-id="8d964-260">A modell és a C makrók használatával megadhatja a **simplesample\_mqtt** mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="8d964-260">You define a model using a set of C macros as in the **simplesample\_mqtt** sample application:</span></span>

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

<span data-ttu-id="8d964-261">A **MEGKEZDÉSÉHEZ\_NÉVTÉR** és **END\_NÉVTÉR** makrók is igénybe vehet a névtér a modell argumentumként.</span><span class="sxs-lookup"><span data-stu-id="8d964-261">The **BEGIN\_NAMESPACE** and **END\_NAMESPACE** macros both take the namespace of the model as an argument.</span></span> <span data-ttu-id="8d964-262">Valószínű, hogy bármi ezek makrók között-e a modell vagy modellek és a adatstruktúrák, amelyek a modellek.</span><span class="sxs-lookup"><span data-stu-id="8d964-262">It's expected that anything between these macros is the definition of your model or models, and the data structures that the models use.</span></span>

<span data-ttu-id="8d964-263">Ebben a példában nincs nevű egyetlen modellt **ContosoAnemometer**.</span><span class="sxs-lookup"><span data-stu-id="8d964-263">In this example, there is a single model called **ContosoAnemometer**.</span></span> <span data-ttu-id="8d964-264">Ez a modell határozza meg, amely az eszköz küldhet az IoT-központ adatok kétféle: **DeviceId** és **szélsebesség**.</span><span class="sxs-lookup"><span data-stu-id="8d964-264">This model defines two pieces of data that your device can send to IoT Hub: **DeviceId** and **WindSpeed**.</span></span> <span data-ttu-id="8d964-265">Az eszköz megkaphatja három műveleteket (üzenet) is definiálja: **TurnFanOn**, **TurnFanOff**, és **SetAirResistance**.</span><span class="sxs-lookup"><span data-stu-id="8d964-265">It also defines three actions (messages) that your device can receive: **TurnFanOn**, **TurnFanOff**, and **SetAirResistance**.</span></span> <span data-ttu-id="8d964-266">Minden adatelemre típussal rendelkezik, és minden művelet nevét (és nem kötelező paraméterek).</span><span class="sxs-lookup"><span data-stu-id="8d964-266">Each data element has a type, and each action has a name (and optionally a set of parameters).</span></span>

<span data-ttu-id="8d964-267">Az adatok és a modellben definiált műveletek határozza meg, hogy üzeneteket küldhet IoT-központot, és az eszközre küldött üzenetek válaszol API felületéhez.</span><span class="sxs-lookup"><span data-stu-id="8d964-267">The data and actions defined in the model define an API surface that you can use to send messages to IoT Hub, and respond to messages sent to the device.</span></span> <span data-ttu-id="8d964-268">Ez a modell használatának legjobb értendő egy példán keresztül.</span><span class="sxs-lookup"><span data-stu-id="8d964-268">Use of this model is best understood through an example.</span></span>

### <a name="send-messages"></a><span data-ttu-id="8d964-269">Üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="8d964-269">Send messages</span></span>

<span data-ttu-id="8d964-270">A modell határozza meg az IoT-központ küldhet adatokat.</span><span class="sxs-lookup"><span data-stu-id="8d964-270">The model defines the data you can send to IoT Hub.</span></span> <span data-ttu-id="8d964-271">Ebben a példában, hogy azt jelenti, hogy egy, a két adatelemek segítségével meghatározott a **WITH_DATA** makró.</span><span class="sxs-lookup"><span data-stu-id="8d964-271">In this example, that means one of the two data items defined using the **WITH_DATA** macro.</span></span> <span data-ttu-id="8d964-272">Nincs több lépésre van szükség a küldési **DeviceId** és **szélsebesség** az IoT-központ értékeket.</span><span class="sxs-lookup"><span data-stu-id="8d964-272">There are several steps required to send **DeviceId** and **WindSpeed** values to an IoT hub.</span></span> <span data-ttu-id="8d964-273">Az első is, hogy az adatokat szeretne küldeni:</span><span class="sxs-lookup"><span data-stu-id="8d964-273">The first is to set the data you want to send:</span></span>

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

<span data-ttu-id="8d964-274">A korábban meghatározott modell lehetővé teszi, hogy meg kell adnia az értékeket úgy, hogy tagjai egy **struct**.</span><span class="sxs-lookup"><span data-stu-id="8d964-274">The model you defined earlier enables you to set the values by setting members of a **struct**.</span></span> <span data-ttu-id="8d964-275">A következő szerializálni az üzenetet szeretne küldeni:</span><span class="sxs-lookup"><span data-stu-id="8d964-275">Next, serialize the message you want to send:</span></span>

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed to serialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

<span data-ttu-id="8d964-276">Ezt a kódot az eszközről-a-felhőbe a pufferbe rendezi sorba (által hivatkozott **cél**).</span><span class="sxs-lookup"><span data-stu-id="8d964-276">This code serializes the device-to-cloud to a buffer (referenced by **destination**).</span></span> <span data-ttu-id="8d964-277">A kódot, majd meghívja a **sendMessage** függvény az üzenetet az IoT hubhoz:</span><span class="sxs-lookup"><span data-stu-id="8d964-277">The code then invokes the **sendMessage** function to send the message to IoT Hub:</span></span>

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable to create a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


<span data-ttu-id="8d964-278">A második utolsó paraméterének **IoTHubClient\_inden\_SendEventAsync** hivatkozás a visszahívási függvénynek, amely nevezzük, amikor az adatok elküldése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="8d964-278">The second to last parameter of **IoTHubClient\_LL\_SendEventAsync** is a reference to a callback function that's called when the data is successfully sent.</span></span> <span data-ttu-id="8d964-279">Itt kell megadni a visszahívási függvény a minta:</span><span class="sxs-lookup"><span data-stu-id="8d964-279">Here's the callback function in the sample:</span></span>

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

<span data-ttu-id="8d964-280">A második paraméter mutató felhasználói környezet; az azonos kapott **IoTHubClient\_inden\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="8d964-280">The second parameter is a pointer to user context; the same pointer passed to **IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="8d964-281">Ebben az esetben a környezetben egy egyszerű számlálót, de bármilyen lehet.</span><span class="sxs-lookup"><span data-stu-id="8d964-281">In this case, the context is a simple counter, but it can be anything you want.</span></span>

<span data-ttu-id="8d964-282">Ez minden, az eszközről a felhőbe üzenetküldésre van.</span><span class="sxs-lookup"><span data-stu-id="8d964-282">That's all there is to sending device-to-cloud messages.</span></span> <span data-ttu-id="8d964-283">Fedik le a bal oldali egyedül, hogyan üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="8d964-283">The only thing left to cover is how to receive messages.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="8d964-284">Hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="8d964-284">Receive messages</span></span>

<span data-ttu-id="8d964-285">Egy üzenet működik mint fogadó üzenetek hogyan működnek a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8d964-285">Receiving a message works similarly to the way messages work in the **IoTHubClient** library.</span></span> <span data-ttu-id="8d964-286">Először regisztrálnia üzenet visszahívási függvény:</span><span class="sxs-lookup"><span data-stu-id="8d964-286">First, you register a message callback function:</span></span>

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable to IoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

<span data-ttu-id="8d964-287">Ezután ír a visszahívási függvény, amelyet ha egy üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8d964-287">Then, you write the callback function that's invoked when a message is received:</span></span>

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

<span data-ttu-id="8d964-288">Ez a kód bolierplate--azonos minden megoldás.</span><span class="sxs-lookup"><span data-stu-id="8d964-288">This code is boilerplate -- it's the same for any solution.</span></span> <span data-ttu-id="8d964-289">Ez a függvény az üzenetet kap, és gondoskodik összegzésére, valamint a megfelelő függvény hívása keresztül **EXECUTE\_parancs**.</span><span class="sxs-lookup"><span data-stu-id="8d964-289">This function receives the message and takes care of routing it to the appropriate function through the call to **EXECUTE\_COMMAND**.</span></span> <span data-ttu-id="8d964-290">A hívott függvény a modellben műveletek definíciója ezen a ponton függ.</span><span class="sxs-lookup"><span data-stu-id="8d964-290">The function called at this point depends on the definition of the actions in your model.</span></span>

<span data-ttu-id="8d964-291">Ha egy műveletet a modell, módosítania nevezzük, amikor az eszköz megkapja a megfelelő üzenetet függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="8d964-291">When you define an action in your model, you're required to implement a function that's called when your device receives the corresponding message.</span></span> <span data-ttu-id="8d964-292">Ha például a modell határozza meg a műveletet:</span><span class="sxs-lookup"><span data-stu-id="8d964-292">For example, if your model defines this action:</span></span>

```c
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="8d964-293">Adja meg a függvény a következő aláírást:</span><span class="sxs-lookup"><span data-stu-id="8d964-293">Define a function with this signature:</span></span>

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="8d964-294">Vegye figyelembe, hogyan függvény neve megegyezik-e a művelet neve, a modell és, hogy a függvény paraméterei egyeznek-e a művelet a megadott paraméter.</span><span class="sxs-lookup"><span data-stu-id="8d964-294">Note how the name of the function matches the name of the action in the model and that the parameters of the function match the parameters specified for the action.</span></span> <span data-ttu-id="8d964-295">Az első paraméter megadása mindig kötelező, és a modell példányának mutatót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8d964-295">The first parameter is always required and contains a pointer to the instance of your model.</span></span>

<span data-ttu-id="8d964-296">Ha az eszköz az aláírásnak megfelelő üzenetet kap, a megfelelő függvény hívása esetén.</span><span class="sxs-lookup"><span data-stu-id="8d964-296">When the device receives a message that matches this signature, the corresponding function is called.</span></span> <span data-ttu-id="8d964-297">Ezért vezérelt kell megadni a bolierplate kódot **IoTHubMessage**, üzenetek fogadására csak egy egyszerű függvény a modellben megadott műveleteket meghatározása.</span><span class="sxs-lookup"><span data-stu-id="8d964-297">Therefore, aside from having to include the boilerplate code from **IoTHubMessage**, receiving messages is just a matter of defining a simple function for each action defined in your model.</span></span>

### <a name="uninitialize-the-library"></a><span data-ttu-id="8d964-298">A könyvtár meghívná</span><span class="sxs-lookup"><span data-stu-id="8d964-298">Uninitialize the library</span></span>

<span data-ttu-id="8d964-299">Amikor elkészült, adatokat küldő és fogadó üzenetek, akkor is meghívná az IoT-könyvtár:</span><span class="sxs-lookup"><span data-stu-id="8d964-299">When you're done sending data and receiving messages, you can uninitialize the IoT library:</span></span>

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

<span data-ttu-id="8d964-300">Mindegyik funkcióhoz három igazodik a korábban ismertetett három inicializálási funkciók.</span><span class="sxs-lookup"><span data-stu-id="8d964-300">Each of these three functions aligns with the three initialization functions described previously.</span></span> <span data-ttu-id="8d964-301">Ezen API-k hívása biztosítja, hogy a korábban kiosztott erőforrásokat ingyenes.</span><span class="sxs-lookup"><span data-stu-id="8d964-301">Calling these APIs ensures that you free previously allocated resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d964-302">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d964-302">Next Steps</span></span>

<span data-ttu-id="8d964-303">Ez a cikk a tárak használatának alapjaival jelez a **C-hez készült SDK Azure IoT-eszközök**. Elegendő információt az SDK tartalma megérteni a megadott, az architektúra és első lépések a Windows-minták használata.</span><span class="sxs-lookup"><span data-stu-id="8d964-303">This article covered the basics of using the libraries in the **Azure IoT device SDK for C**. It provided you with enough information to understand what's included in the SDK, its architecture, and how to get started working with the Windows samples.</span></span> <span data-ttu-id="8d964-304">A következő cikk leírja, hogy az SDK által, amely ismerteti, hogy továbbra is fennáll. [további információk a IoTHubClient könyvtár](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="8d964-304">The next article continues the description of the SDK by explaining [more about the IoTHubClient library](iot-hub-device-sdk-c-iothubclient.md).</span></span>

<span data-ttu-id="8d964-305">Az IoT-központ fejlesztésével kapcsolatos további tudnivalókért tekintse meg a [Azure IoT SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="8d964-305">To learn more about developing for IoT Hub, see the [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="8d964-306">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="8d964-306">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8d964-307">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="8d964-307">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
