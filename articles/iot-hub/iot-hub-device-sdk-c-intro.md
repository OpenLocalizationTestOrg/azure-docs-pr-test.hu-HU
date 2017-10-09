---
title: "aaaThe Azure IoT-eszközök SDK C-hez |} Microsoft Docs"
description: "Ismerkedés a hello Azure IoT-eszközök SDK C-hez, és megtudhatja, hogyan toocreate eszközeinek alkalmazásait, amely kommunikálni az IoT-központ."
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
ms.openlocfilehash: 9e20742e6ea513c124bfaf28f02f6fba86170daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c"></a><span data-ttu-id="2a74e-103">Az Azure IoT-eszközök SDK C-hez</span><span class="sxs-lookup"><span data-stu-id="2a74e-103">Azure IoT device SDK for C</span></span>

<span data-ttu-id="2a74e-104">Hello **Azure IoT-eszközök SDK** szalagtárak készlete tervezett toosimplify hello folyamat tooand fogadó üzenetek üzenetet küldeni a hello **Azure IoT Hub** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2a74e-104">hello **Azure IoT device SDK** is a set of libraries designed toosimplify hello process of sending messages tooand receiving messages from hello **Azure IoT Hub** service.</span></span> <span data-ttu-id="2a74e-105">SDK-t és egy adott platform célzó hello eltérő változata van, de ez a cikk ismerteti a hello **C-hez készült SDK Azure IoT-eszközök**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-105">There are different variations of hello SDK, each targeting a specific platform, but this article describes hello **Azure IoT device SDK for C**.</span></span>

<span data-ttu-id="2a74e-106">hello Azure IoT-eszközök SDK c ANSI C (C99) toomaximize hordozhatósága nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="2a74e-106">hello Azure IoT device SDK for C is written in ANSI C (C99) toomaximize portability.</span></span> <span data-ttu-id="2a74e-107">A szolgáltatás révén hello szalagtárak jól alkalmazható toooperate több platformokon és eszközökön, különösen akkor, ha minimalizálja a lemez és a memóriaigény prioritását.</span><span class="sxs-lookup"><span data-stu-id="2a74e-107">This feature makes hello libraries well-suited toooperate on multiple platforms and devices, especially where minimizing disk and memory footprint is a priority.</span></span>

<span data-ttu-id="2a74e-108">Nincsenek mely hello az SDK-t tesztelték platformok széles körének (lásd: hello [Azure IoT eszköz katalógus hitelesített](https://catalog.azureiotsuite.com/) részletekért).</span><span class="sxs-lookup"><span data-stu-id="2a74e-108">There are a broad range of platforms on which hello SDK has been tested (see hello [Azure Certified for IoT device catalog](https://catalog.azureiotsuite.com/) for details).</span></span> <span data-ttu-id="2a74e-109">Bár ez a cikk a Windows hello platformon futó mintakód forgatókönyvek tartalmaz, akkor hello kód cikkben leírt megegyezik hello támogatott platformok tartományán keresztül.</span><span class="sxs-lookup"><span data-stu-id="2a74e-109">Although this article includes walkthroughs of sample code running on hello Windows platform, hello code described in this article is identical across hello range of supported platforms.</span></span>

<span data-ttu-id="2a74e-110">Ez a cikk bemutatja a toohello architektúra az hello Azure IoT-eszközök SDK a c kiszolgálóra. Azt mutatja be, hogyan tooinitialize hello hálózatieszköz-könyvtár adatok tooIoT Hub, üzeneteket küldjön és fogadjon abból.</span><span class="sxs-lookup"><span data-stu-id="2a74e-110">This article introduces you toohello architecture of hello Azure IoT device SDK for C. It demonstrates how tooinitialize hello device library, send data tooIoT Hub, and receive messages from it.</span></span> <span data-ttu-id="2a74e-111">a cikkben szereplő információkat hello legyen elég tooget hello SDK használatának, de hello szalagtárak mutatók tooadditional adatait is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2a74e-111">hello information in this article should be enough tooget started using hello SDK, but also provides pointers tooadditional information about hello libraries.</span></span>

## <a name="sdk-architecture"></a><span data-ttu-id="2a74e-112">SDK-architektúra</span><span class="sxs-lookup"><span data-stu-id="2a74e-112">SDK architecture</span></span>

<span data-ttu-id="2a74e-113">Hello található [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) hello API hello a GitHub tárház és nézet adatai [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="2a74e-113">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

<span data-ttu-id="2a74e-114">hello hello könyvtárak legújabb verziója található hello **fő** hello tárház főágába:</span><span class="sxs-lookup"><span data-stu-id="2a74e-114">hello latest version of hello libraries can be found in hello **master** branch of hello repository:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* <span data-ttu-id="2a74e-115">hello hello SDK alapvető végrehajtásának van hello **IOT hubbal\_ügyfél** hello legalacsonyabb API réteg hello SDK hello végrehajtásának tartalmazó mappa: hello **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2a74e-115">hello core implementation of hello SDK is in hello **iothub\_client** folder that contains hello implementation of hello lowest API layer in hello SDK: hello **IoTHubClient** library.</span></span> <span data-ttu-id="2a74e-116">Hello **IoTHubClient** könyvtár végrehajtási üzenetek tooIoT Hub üzenetek küldése és fogadása az IoT-központ az üzenetküldési nyers API-kat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2a74e-116">hello **IoTHubClient** library contains APIs implementing raw messaging for sending messages tooIoT Hub and receiving messages from IoT Hub.</span></span> <span data-ttu-id="2a74e-117">Ha ezt a szalagtárat, üzenet szerializálási végrehajtásáért felelős, de egyéb adatainak kommunikál az IoT-központ történik meg.</span><span class="sxs-lookup"><span data-stu-id="2a74e-117">When using this library, you are responsible for implementing message serialization, but other details of communicating with IoT Hub are handled for you.</span></span>
* <span data-ttu-id="2a74e-118">Hello **szerializáló** mappája súgófunkciókat és minták, amelyek bemutatják, hogyan tooserialize adatok küldése a tooAzure IoT Hub használata előtt hello ügyféloldali kódtára tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2a74e-118">hello **serializer** folder contains helper functions and samples that show you how tooserialize data before sending tooAzure IoT Hub using hello client library.</span></span> <span data-ttu-id="2a74e-119">hello szerializáló hello használata nem kötelező, és a könnyebb biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2a74e-119">hello use of hello serializer is not mandatory and is provided as a convenience.</span></span> <span data-ttu-id="2a74e-120">toouse hello **szerializáló** könyvtár, egy modell, amely meghatározza az adatok toosend tooIoT Hub és hello köszönőüzenetei tooreceive származó várt megadása.</span><span class="sxs-lookup"><span data-stu-id="2a74e-120">toouse hello **serializer** library, you define a model that specifies hello data toosend tooIoT Hub and hello messages you expect tooreceive from it.</span></span> <span data-ttu-id="2a74e-121">Miután hello modell van definiálva, hello SDK használható biztosít, amely lehetővé teszi API felületéhez tooeasily az eszközről a felhőbe, és anélkül, hogy a felhő eszközre üzenetek hello szerializálási részleteit.</span><span class="sxs-lookup"><span data-stu-id="2a74e-121">Once hello model is defined, hello SDK provides you with an API surface that enables you tooeasily work with device-to-cloud and cloud-to-device messages without worrying about hello serialization details.</span></span> <span data-ttu-id="2a74e-122">hello könyvtár más nyílt forráskódú tárak, amelyek megvalósítják az átviteli protokollal például MQTT és AMQP függ.</span><span class="sxs-lookup"><span data-stu-id="2a74e-122">hello library depends on other open source libraries that implement transport using protocols such as MQTT and AMQP.</span></span>
* <span data-ttu-id="2a74e-123">Hello **IoTHubClient** könyvtár más nyílt forráskódú szalagtárak függ:</span><span class="sxs-lookup"><span data-stu-id="2a74e-123">hello **IoTHubClient** library depends on other open source libraries:</span></span>
  * <span data-ttu-id="2a74e-124">Hello [Azure C megosztott segédprogram](https://github.com/Azure/azure-c-shared-utility) könyvtárban, amely több Azure-hoz kapcsolódó C SDK között szükséges alapvető feladatokhoz (például karakterláncok, lista manipuláció és IO) közös funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="2a74e-124">hello [Azure C shared utility](https://github.com/Azure/azure-c-shared-utility) library, which provides common functionality for basic tasks (such as strings, list manipulation, and IO) needed across several Azure-related C SDKs.</span></span>
  * <span data-ttu-id="2a74e-125">Hello [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) könyvtárban, és azt egy AMQP korlátozott erőforrás eszközök optimalizálva ügyféloldali megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="2a74e-125">hello [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) library, which is a client-side implementation of AMQP optimized for resource constrained devices.</span></span>
  * <span data-ttu-id="2a74e-126">Hello [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) könyvtárba, amely egy általános célú függvénytár végrehajtási hello MQTT protokoll, és korlátozott erőforrás eszközök optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="2a74e-126">hello [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) library, which is a general-purpose library implementing hello MQTT protocol and optimized for resource constrained devices.</span></span>

<span data-ttu-id="2a74e-127">Ezek a könyvtárak használata könnyebb toounderstand kódpéldákat megnézzük.</span><span class="sxs-lookup"><span data-stu-id="2a74e-127">Use of these libraries is easier toounderstand by looking at example code.</span></span> <span data-ttu-id="2a74e-128">a következő szakaszok hello végigvezetik Önt hello alkalmazásokra, amelyek szerepelnek a hello SDK számos.</span><span class="sxs-lookup"><span data-stu-id="2a74e-128">hello following sections walk you through several of hello sample applications that are included in hello SDK.</span></span> <span data-ttu-id="2a74e-129">Ez a forgatókönyv adjon meg helyes érzi, hogy a hello hello architekturális rétegek hello SDK és az API-k működnek bemutatása toohow hello különböző képességeire.</span><span class="sxs-lookup"><span data-stu-id="2a74e-129">This walkthrough should give you a good feel for hello various capabilities of hello architectural layers of hello SDK and an introduction toohow hello APIs work.</span></span>

## <a name="before-you-run-hello-samples"></a><span data-ttu-id="2a74e-130">Mielőtt újra lefuttatja hello minták</span><span class="sxs-lookup"><span data-stu-id="2a74e-130">Before you run hello samples</span></span>

<span data-ttu-id="2a74e-131">Hello Azure IoT-eszközök SDK hello minták c futtathat, előtt [hello IoT-központ szolgáltatás példányának létrehozása](iot-hub-create-through-portal.md) az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="2a74e-131">Before you can run hello samples in hello Azure IoT device SDK for C, you must [create an instance of hello IoT Hub service](iot-hub-create-through-portal.md) in your Azure subscription.</span></span> <span data-ttu-id="2a74e-132">Fejezze be a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="2a74e-132">Then complete hello following tasks:</span></span>

* <span data-ttu-id="2a74e-133">A fejlesztőkörnyezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="2a74e-133">Prepare your development environment</span></span>
* <span data-ttu-id="2a74e-134">Szerezze be a hitelesítő adatai.</span><span class="sxs-lookup"><span data-stu-id="2a74e-134">Obtain device credentials.</span></span>

### <a name="prepare-your-development-environment"></a><span data-ttu-id="2a74e-135">A fejlesztőkörnyezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="2a74e-135">Prepare your development environment</span></span>

<span data-ttu-id="2a74e-136">Csomagok (például NuGet Windows vagy apt_get Debian és Ubuntu) közös platformok kapnak, és hello minták használja ezeket a csomagokat, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="2a74e-136">Packages are provided for common platforms (such as NuGet for Windows or apt_get for Debian and Ubuntu) and hello samples use these packages when available.</span></span> <span data-ttu-id="2a74e-137">Bizonyos esetekben kell toocompile hello SDK számára, vagy az eszközön.</span><span class="sxs-lookup"><span data-stu-id="2a74e-137">In some cases, you need toocompile hello SDK for or on your device.</span></span> <span data-ttu-id="2a74e-138">Ha toocompile hello SDK van szüksége, tekintse meg [a fejlesztőkörnyezet előkészítése](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="2a74e-138">If you need toocompile hello SDK, see [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) in hello GitHub repository.</span></span>

<span data-ttu-id="2a74e-139">tooobtain hello alkalmazás kódmintában letöltési hello SDK a Githubon verzióját.</span><span class="sxs-lookup"><span data-stu-id="2a74e-139">tooobtain hello sample application code, download a copy of hello SDK from GitHub.</span></span> <span data-ttu-id="2a74e-140">Hello forrás példányának lekérése hello **fő** hello ága [GitHub-tárházban](https://github.com/Azure/azure-iot-sdk-c).</span><span class="sxs-lookup"><span data-stu-id="2a74e-140">Get your copy of hello source from hello **master** branch of hello [GitHub repository](https://github.com/Azure/azure-iot-sdk-c).</span></span>


### <a name="obtain-hello-device-credentials"></a><span data-ttu-id="2a74e-141">Hello eszköz hitelesítő adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="2a74e-141">Obtain hello device credentials</span></span>

<span data-ttu-id="2a74e-142">Most, hogy hello minta forráskódját, hello következő lépésként toodo tooget eszköz hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a74e-142">Now that you have hello sample source code, hello next thing toodo is tooget a set of device credentials.</span></span> <span data-ttu-id="2a74e-143">Egy eszköz toobe képes tooaccess az IoT-központ, a hozzá kell adnia hello eszköz toohello IoT-központ identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="2a74e-143">For a device toobe able tooaccess an IoT hub, you must first add hello device toohello IoT Hub identity registry.</span></span> <span data-ttu-id="2a74e-144">Az eszköz hozzáadásakor kap az hello eszköz toobe képes tooconnect toohello IoT-központ szükséges eszköz hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a74e-144">When you add your device, you get a set of device credentials that you need for hello device toobe able tooconnect toohello IoT hub.</span></span> <span data-ttu-id="2a74e-145">hello a következő szakaszban tárgyalt hello mintaalkalmazások ezeket a hitelesítő adatokat várt hello formája egy **eszköz kapcsolati karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-145">hello sample applications discussed in hello next section expect these credentials in hello form of a **device connection string**.</span></span>

<span data-ttu-id="2a74e-146">Számos nyílt forráskódú eszközök toohelp kezelheti az IoT hub van.</span><span class="sxs-lookup"><span data-stu-id="2a74e-146">There are several open source tools toohelp you manage your IoT hub.</span></span>

* <span data-ttu-id="2a74e-147">Egy nevű Windows-alkalmazás [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="2a74e-147">A Windows application called [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>
* <span data-ttu-id="2a74e-148">A platformok közötti node.js parancssori eszköz neve [IOT hubbal-explorer](https://github.com/azure/iothub-explorer).</span><span class="sxs-lookup"><span data-stu-id="2a74e-148">A cross-platform node.js CLI tool called [iothub-explorer](https://github.com/azure/iothub-explorer).</span></span>

<span data-ttu-id="2a74e-149">Ez az oktatóanyag használja grafikus hello *eszköz explorer* eszköz.</span><span class="sxs-lookup"><span data-stu-id="2a74e-149">This tutorial uses hello graphical *device explorer* tool.</span></span> <span data-ttu-id="2a74e-150">Is használhatja a hello *IOT hubbal-explorer* eszköze, ha jobban szeret toouse parancssori eszközt.</span><span class="sxs-lookup"><span data-stu-id="2a74e-150">You can also use hello *iothub-explorer* tool if you prefer toouse a CLI tool.</span></span>

<span data-ttu-id="2a74e-151">hello eszköz explorer eszköz különböző funkciókat használ hello Azure IoT szolgáltatás szalagtárak tooperform az IoT-központot, beleértve az eszközök.</span><span class="sxs-lookup"><span data-stu-id="2a74e-151">hello device explorer tool uses hello Azure IoT service libraries tooperform various functions on IoT Hub, including adding devices.</span></span> <span data-ttu-id="2a74e-152">Hello eszköz explorer eszköz tooadd eszköz használatakor az eszközhöz kapott egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2a74e-152">If you use hello device explorer tool tooadd a device, you get a connection string for your device.</span></span> <span data-ttu-id="2a74e-153">A kapcsolati karakterlánc toorun hello mintaalkalmazások van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2a74e-153">You need this connection string toorun hello sample applications.</span></span>

<span data-ttu-id="2a74e-154">Ha nem ismeri a hello eszköz explorer eszközzel, a következő eljárás hello ismerteti hogyan toouse azt tooadd egy eszköz, és szerezze be az eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2a74e-154">If you're not familiar with hello device explorer tool, hello following procedure describes how toouse it tooadd a device and obtain a device connection string.</span></span>

<span data-ttu-id="2a74e-155">tooinstall hello eszköz explorer eszköz, lásd: [toouse hogyan eszköz Explorer hello IoT Hub-eszközöknek](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="2a74e-155">tooinstall hello device explorer tool, see [How toouse hello Device Explorer for IoT Hub devices](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>

<span data-ttu-id="2a74e-156">Hello program futtatásakor ez az interfész jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2a74e-156">When you run hello program, you see this interface:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

<span data-ttu-id="2a74e-157">Adja meg a **IoT Hub kapcsolati karakterlánc** hello az első mezőben, majd kattintson **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-157">Enter your **IoT Hub Connection String** in hello first field and click **Update**.</span></span> <span data-ttu-id="2a74e-158">Ez a lépés hello eszköz konfigurálja, hogy kommunikáljon az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2a74e-158">This step configures hello tool so that it can communicate with IoT Hub.</span></span>

<span data-ttu-id="2a74e-159">Az IoT-központ kapcsolati karakterlánc hello konfigurálását, kattintson hello **felügyeleti** lapon:</span><span class="sxs-lookup"><span data-stu-id="2a74e-159">When hello IoT Hub connection string is configured, click hello **Management** tab:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

<span data-ttu-id="2a74e-160">Ez a lap majdnem, amelyeken kezelheti az IoT hub-ben regisztrált hello eszközök.</span><span class="sxs-lookup"><span data-stu-id="2a74e-160">This tab is where you manage hello devices registered in your IoT hub.</span></span>

<span data-ttu-id="2a74e-161">Hello kattintva hozhat létre egy eszköz **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="2a74e-161">You create a device by clicking hello **Create** button.</span></span> <span data-ttu-id="2a74e-162">Olyan előre megadott kulcsokat (elsődleges és másodlagos) egy párbeszédpanelt jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="2a74e-162">A dialog displays with a set of pre-populated keys (primary and secondary).</span></span> <span data-ttu-id="2a74e-163">Adjon meg egy **Eszközazonosító** majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-163">Enter a **Device ID** and then click **Create**.</span></span>

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

<span data-ttu-id="2a74e-164">Hello eszköz létrehozásakor hello eszközök frissítések minden hello regisztrált eszközökkel, például egy újonnan létrehozott hello listában.</span><span class="sxs-lookup"><span data-stu-id="2a74e-164">When hello device is created, hello Devices list updates with all hello registered devices, including hello one you just created.</span></span> <span data-ttu-id="2a74e-165">Az új eszköz a jobb gombbal, az ebben a menüben jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2a74e-165">If you right-click your new device, you see this menu:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

<span data-ttu-id="2a74e-166">Ha úgy dönt, **másolja a kijelölt eszközre vonatkozóan a kapcsolati karakterlánc**, hello eszköz kapcsolati karakterláncot a vágólapra másolt toohello.</span><span class="sxs-lookup"><span data-stu-id="2a74e-166">If you choose **Copy connection string for selected device**, hello device connection string is copied toohello clipboard.</span></span> <span data-ttu-id="2a74e-167">Hello eszköz kapcsolati karakterlánc egy példányát megőrizni.</span><span class="sxs-lookup"><span data-stu-id="2a74e-167">Keep a copy of hello device connection string.</span></span> <span data-ttu-id="2a74e-168">Meg kell hello a következő részekben leírt hello mintaalkalmazások futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="2a74e-168">You need it when running hello sample applications described in hello following sections.</span></span>

<span data-ttu-id="2a74e-169">Hello fenti lépések végrehajtását, hamarosan kész toostart bizonyos kód futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2a74e-169">When you've completed hello steps above, you're ready toostart running some code.</span></span> <span data-ttu-id="2a74e-170">Mindkét minták állandó rendelkezik, amely lehetővé teszi egy kapcsolati karakterláncot tooenter hello fő forrásfájl hello tetején.</span><span class="sxs-lookup"><span data-stu-id="2a74e-170">Both samples have a constant at hello top of hello main source file that enables you tooenter a connection string.</span></span> <span data-ttu-id="2a74e-171">Például a megfelelő sor a hello hello **IOT hubbal\_ügyfél\_minta\_mqtt** alkalmazás a következőképpen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2a74e-171">For example, hello corresponding line from hello **iothub\_client\_sample\_mqtt** application appears as follows.</span></span>

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-hello-iothubclient-library"></a><span data-ttu-id="2a74e-172">Hello IoTHubClient könyvtárban</span><span class="sxs-lookup"><span data-stu-id="2a74e-172">Use hello IoTHubClient library</span></span>

<span data-ttu-id="2a74e-173">Hello belül **IOT hubbal\_ügyfél** hello mappájában [azure iot-sdk--c](https://github.com/azure/azure-iot-sdk-c) -tárházban, van egy **minták** mappába, amelyben egy alkalmazás nevű **IOT hubbal\_ügyfél\_minta\_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-173">Within hello **iothub\_client** folder in hello [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) repository, there is a **samples** folder that contains an application called **iothub\_client\_sample\_mqtt**.</span></span>

<span data-ttu-id="2a74e-174">Windows-verzión hello hello **IOT hubbal\_ügyfél\_minta\_mqtt** alkalmazás tartalmazza a Visual Studio megoldás a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a74e-174">hello Windows version of hello **iothub\_client\_sample\_mqtt** application includes hello following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="2a74e-175">Ha megnyitja a projektet a Visual Studio 2017, fogadja el a hello kér tooretarget hello projekt toohello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="2a74e-175">If you open this project in Visual Studio 2017, accept hello prompts tooretarget hello project toohello latest version.</span></span>

<span data-ttu-id="2a74e-176">Ez a megoldás egyetlen projektet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2a74e-176">This solution contains a single project.</span></span> <span data-ttu-id="2a74e-177">Nincsenek telepítve az ebben a megoldásban négy NuGet-csomagok:</span><span class="sxs-lookup"><span data-stu-id="2a74e-177">There are four NuGet packages installed in this solution:</span></span>

* <span data-ttu-id="2a74e-178">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="2a74e-178">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="2a74e-179">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="2a74e-179">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="2a74e-180">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="2a74e-180">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="2a74e-181">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="2a74e-181">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="2a74e-182">Folyamatosan hello **Microsoft.Azure.C.SharedUtility** amikor dolgozunk hello SDK csomagot.</span><span class="sxs-lookup"><span data-stu-id="2a74e-182">You always need hello **Microsoft.Azure.C.SharedUtility** package when you are working with hello SDK.</span></span> <span data-ttu-id="2a74e-183">Ez a minta hello MQTT protokollt használja, ezért meg kell adni hello **Microsoft.Azure.umqtt** és **Microsoft.Azure.IoTHub.MqttTransport** (nincsenek egyenértékű csomagok AMQP és HTTP csomagok ).</span><span class="sxs-lookup"><span data-stu-id="2a74e-183">This sample uses hello MQTT protocol, therefore you must include hello **Microsoft.Azure.umqtt** and **Microsoft.Azure.IoTHub.MqttTransport** packages (there are equivalent packages for AMQP and HTTP).</span></span> <span data-ttu-id="2a74e-184">Mivel hello mintát használ hello **IoTHubClient** könyvtár, is magában kell foglalnia hello **Microsoft.Azure.IoTHub.IoTHubClient** csomag a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="2a74e-184">Because hello sample uses hello **IoTHubClient** library, you must also include hello **Microsoft.Azure.IoTHub.IoTHubClient** package in your solution.</span></span>

<span data-ttu-id="2a74e-185">Hello megvalósítási hello mintaalkalmazás hello található **IOT hubbal\_ügyfél\_minta\_mqtt.c** forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="2a74e-185">You can find hello implementation for hello sample application in hello **iothub\_client\_sample\_mqtt.c** source file.</span></span>

<span data-ttu-id="2a74e-186">hello következő lépések használják a minta alkalmazás toowalk le toouse hello mi szükséges a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2a74e-186">hello following steps use this sample application toowalk you through what's required toouse hello **IoTHubClient** library.</span></span>

### <a name="initialize-hello-library"></a><span data-ttu-id="2a74e-187">Hello kódtár inicializálása</span><span class="sxs-lookup"><span data-stu-id="2a74e-187">Initialize hello library</span></span>

> [!NOTE]
> <span data-ttu-id="2a74e-188">Mielőtt az hello szalagtárak használatának megkezdése, szükséges tooperform néhány platform-specifikus inicializálása.</span><span class="sxs-lookup"><span data-stu-id="2a74e-188">Before you start working with hello libraries, you may need tooperform some platform-specific initialization.</span></span> <span data-ttu-id="2a74e-189">Ha azt tervezi, toouse AMQP Linux például kell hello OpenSSL kódtár inicializálása.</span><span class="sxs-lookup"><span data-stu-id="2a74e-189">For example, if you plan toouse AMQP on Linux you must initialize hello OpenSSL library.</span></span> <span data-ttu-id="2a74e-190">hello minták hello [GitHub-tárházban](https://github.com/Azure/azure-iot-sdk-c) hello segédprogram függvény **platform\_init** amikor hello ügyfél elindul, és hívja meg hello **platform\_deinit**  függvény való kilépés előtt.</span><span class="sxs-lookup"><span data-stu-id="2a74e-190">hello samples in hello [GitHub repository](https://github.com/Azure/azure-iot-sdk-c) call hello utility function **platform\_init** when hello client starts and call hello **platform\_deinit** function before exiting.</span></span> <span data-ttu-id="2a74e-191">Ezek a funkciók hello platform.h fejlécfájlt van deklarálva.</span><span class="sxs-lookup"><span data-stu-id="2a74e-191">These functions are declared in hello platform.h header file.</span></span> <span data-ttu-id="2a74e-192">Vizsgálja meg ezeket a funkciókat a célplatformot a hello hello definíciók [tárház](https://github.com/Azure/azure-iot-sdk-c) toodetermine e szükséges tooinclude bármely platform-specifikus inicializálási kód az ügyfél.</span><span class="sxs-lookup"><span data-stu-id="2a74e-192">Examine hello definitions of these functions for your target platform in hello [repository](https://github.com/Azure/azure-iot-sdk-c) toodetermine whether you need tooinclude any platform-specific initialization code in your client.</span></span>

<span data-ttu-id="2a74e-193">hello könyvtárakkal toostart először lefoglalni egy IoT-központ ügyfél leíró:</span><span class="sxs-lookup"><span data-stu-id="2a74e-193">toostart working with hello libraries, first allocate an IoT Hub client handle:</span></span>

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

<span data-ttu-id="2a74e-194">Hello eszköz kapcsolati karakterláncot kapott, hello eszköz explorer eszköz toothis függvény másolatát át.</span><span class="sxs-lookup"><span data-stu-id="2a74e-194">You pass a copy of hello device connection string you obtained from hello device explorer tool toothis function.</span></span> <span data-ttu-id="2a74e-195">Is megjelölhetők hello kommunikációs protokollt toouse.</span><span class="sxs-lookup"><span data-stu-id="2a74e-195">You also designate hello communications protocol toouse.</span></span> <span data-ttu-id="2a74e-196">Ez a példa MQTT, de amqp-t és HTTP is lehetősége.</span><span class="sxs-lookup"><span data-stu-id="2a74e-196">This example uses MQTT, but AMQP and HTTP are also options.</span></span>

<span data-ttu-id="2a74e-197">Ha rendelkezik olyan érvényes **IOT HUBBAL\_ügyfél\_KEZELNI**, hívja az API-k toosend hello elindíthatja és tooand üzenetek fogadása az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2a74e-197">When you have a valid **IOTHUB\_CLIENT\_HANDLE**, you can start calling hello APIs toosend and receive messages tooand from IoT Hub.</span></span>

### <a name="send-messages"></a><span data-ttu-id="2a74e-198">Üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="2a74e-198">Send messages</span></span>

<span data-ttu-id="2a74e-199">hello mintaalkalmazás állít be egy hurok toosend üzenetek tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2a74e-199">hello sample application sets up a loop toosend messages tooyour IoT hub.</span></span> <span data-ttu-id="2a74e-200">a következő kódrészletet hello:</span><span class="sxs-lookup"><span data-stu-id="2a74e-200">hello following snippet:</span></span>

- <span data-ttu-id="2a74e-201">Létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="2a74e-201">Creates a message.</span></span>
- <span data-ttu-id="2a74e-202">Hozzáad egy tulajdonság toohello üzenetet.</span><span class="sxs-lookup"><span data-stu-id="2a74e-202">Adds a property toohello message.</span></span>
- <span data-ttu-id="2a74e-203">Üzenet küldése.</span><span class="sxs-lookup"><span data-stu-id="2a74e-203">Sends a message.</span></span>

<span data-ttu-id="2a74e-204">Először hozzon létre egy üzenetet:</span><span class="sxs-lookup"><span data-stu-id="2a74e-204">First, create a message:</span></span>

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
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission tooIoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

<span data-ttu-id="2a74e-205">Minden alkalommal, amikor egy üzenetet küld, meg kell adnia egy referencia tooa visszahívási függvény hello adatküldést hív meg.</span><span class="sxs-lookup"><span data-stu-id="2a74e-205">Every time you send a message, you specify a reference tooa callback function that's invoked when hello data is sent.</span></span> <span data-ttu-id="2a74e-206">Ebben a példában a hello visszahívási függvény hívása esetén **SendConfirmationCallback**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-206">In this example, hello callback function is called **SendConfirmationCallback**.</span></span> <span data-ttu-id="2a74e-207">a következő kódrészletet hello a visszahívási függvény jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="2a74e-207">hello following snippet shows this callback function:</span></span>

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

<span data-ttu-id="2a74e-208">Vegye figyelembe a hello hívás toohello **IoTHubMessage\_Destroy** működik, ha üdvözlőüzenetére végzett.</span><span class="sxs-lookup"><span data-stu-id="2a74e-208">Note hello call toohello **IoTHubMessage\_Destroy** function when you're done with hello message.</span></span> <span data-ttu-id="2a74e-209">Ez a funkció ennyi helyet szabadít hello erőforrások lefoglalt üdvözlőüzenetére létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2a74e-209">This function frees hello resources allocated when you created hello message.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="2a74e-210">Hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="2a74e-210">Receive messages</span></span>

<span data-ttu-id="2a74e-211">Egy üzenet fogadását egy aszinkron művelet.</span><span class="sxs-lookup"><span data-stu-id="2a74e-211">Receiving a message is an asynchronous operation.</span></span> <span data-ttu-id="2a74e-212">Először hello visszahívási tooinvoke regisztrálása, amikor hello eszköz üzenetet kapja:</span><span class="sxs-lookup"><span data-stu-id="2a74e-212">First, you register hello callback tooinvoke when hello device receives a message:</span></span>

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

<span data-ttu-id="2a74e-213">hello utolsó paraméter értéke a "void" mutató toowhatever szeretné.</span><span class="sxs-lookup"><span data-stu-id="2a74e-213">hello last parameter is a void pointer toowhatever you want.</span></span> <span data-ttu-id="2a74e-214">Hello mintát, az egész számnak mutató tooan is, de ez lehet egy mutató tooa összetettebb adatstruktúra.</span><span class="sxs-lookup"><span data-stu-id="2a74e-214">In hello sample, it's a pointer tooan integer but it could be a pointer tooa more complex data structure.</span></span> <span data-ttu-id="2a74e-215">Ez a paraméter lehetővé teszi, hogy a hello visszahívási függvény toooperate a függvény hello hívó rendelkező megosztott állapot.</span><span class="sxs-lookup"><span data-stu-id="2a74e-215">This parameter enables hello callback function toooperate on shared state with hello caller of this function.</span></span>

<span data-ttu-id="2a74e-216">Hello eszköz üzenetet kap, amikor hello regisztrált visszahívási függvényt hívják.</span><span class="sxs-lookup"><span data-stu-id="2a74e-216">When hello device receives a message, hello registered callback function is invoked.</span></span> <span data-ttu-id="2a74e-217">A visszahívási függvény kéri le:</span><span class="sxs-lookup"><span data-stu-id="2a74e-217">This callback function retrieves:</span></span>

* <span data-ttu-id="2a74e-218">hello üzenetazonosítója és hello üzenet korrelációs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2a74e-218">hello message id and correlation id from hello message.</span></span>
* <span data-ttu-id="2a74e-219">hello üzenet tartalma.</span><span class="sxs-lookup"><span data-stu-id="2a74e-219">hello message content.</span></span>
* <span data-ttu-id="2a74e-220">Egyéni tulajdonságok hello üzenetből.</span><span class="sxs-lookup"><span data-stu-id="2a74e-220">Any custom properties from hello message.</span></span>

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
        (void)printf("unable tooretrieve hello message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive hello work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from hello message
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

<span data-ttu-id="2a74e-221">Használjon hello **IoTHubMessage\_GetByteArray** függvény tooretrieve hello üzenet, amely ebben a példában a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2a74e-221">Use hello **IoTHubMessage\_GetByteArray** function tooretrieve hello message, which in this example is a string.</span></span>

### <a name="uninitialize-hello-library"></a><span data-ttu-id="2a74e-222">Meghívná hello könyvtár</span><span class="sxs-lookup"><span data-stu-id="2a74e-222">Uninitialize hello library</span></span>

<span data-ttu-id="2a74e-223">Ha elkészült a küldő események, és fogadja az üzeneteket, hello IoT könyvtár is meghívná.</span><span class="sxs-lookup"><span data-stu-id="2a74e-223">When you're done sending events and receiving messages, you can uninitialize hello IoT library.</span></span> <span data-ttu-id="2a74e-224">toodo Igen, a következő függvény hívásához szükséges hello adja ki:</span><span class="sxs-lookup"><span data-stu-id="2a74e-224">toodo so, issue hello following function call:</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="2a74e-225">A hívás szabaddá korábban le van foglalva hello hello erőforrások **IoTHubClient\_CreateFromConnectionString** függvény.</span><span class="sxs-lookup"><span data-stu-id="2a74e-225">This call frees up hello resources previously allocated by hello **IoTHubClient\_CreateFromConnectionString** function.</span></span>

<span data-ttu-id="2a74e-226">Ahogy látja, könnyen toosend és üzenetek fogadása az hello **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2a74e-226">As you can see, it's easy toosend and receive messages with hello **IoTHubClient** library.</span></span> <span data-ttu-id="2a74e-227">hello könyvtár kezeli az IoT-központot, mely protokoll toouse beleértve folytatott kommunikáció hello részleteit (hello fejlesztői hello szempontjából, ez a lehetőség egy egyszerű konfiguráció).</span><span class="sxs-lookup"><span data-stu-id="2a74e-227">hello library handles hello details of communicating with IoT Hub, including which protocol toouse (from hello perspective of hello developer, this is a simple configuration option).</span></span>

<span data-ttu-id="2a74e-228">Hello **IoTHubClient** kódtár is biztosít a pontos szabályozhatják, hogyan tooserialize hello adatok az eszköz küld tooIoT központ.</span><span class="sxs-lookup"><span data-stu-id="2a74e-228">hello **IoTHubClient** library also provides precise control over how tooserialize hello data your device sends tooIoT Hub.</span></span> <span data-ttu-id="2a74e-229">Egyes esetekben ez az érték előnyös, de az mások számára nem szeretné, hogy az érintett toobe egy megvalósítási részletek is.</span><span class="sxs-lookup"><span data-stu-id="2a74e-229">In some cases this level of control is an advantage, but in others it is an implementation detail that you don't want toobe concerned with.</span></span> <span data-ttu-id="2a74e-230">Ha hello esetben érdemes, a hello **szerializáló** hello a következő szakaszban ismertetett könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2a74e-230">If that's hello case, you might consider using hello **serializer** library, which is described in hello next section.</span></span>

## <a name="use-hello-serializer-library"></a><span data-ttu-id="2a74e-231">Hello szerializáló könyvtárban</span><span class="sxs-lookup"><span data-stu-id="2a74e-231">Use hello serializer library</span></span>

<span data-ttu-id="2a74e-232">Fogalmilag hello **szerializáló** könyvtárban található fölött hello **IoTHubClient** hello SDK szalagtár.</span><span class="sxs-lookup"><span data-stu-id="2a74e-232">Conceptually hello **serializer** library sits on top of hello **IoTHubClient** library in hello SDK.</span></span> <span data-ttu-id="2a74e-233">Hello használ **IoTHubClient** könyvtár a kommunikáció az IoT-központ, de a mögöttes hello hello terheket üzenet szerializálási való eltávolításához hello fejlesztőtől származó modellezési lényeges képességét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2a74e-233">It uses hello **IoTHubClient** library for hello underlying communication with IoT Hub, but it adds modeling capabilities that remove hello burden of dealing with message serialization from hello developer.</span></span> <span data-ttu-id="2a74e-234">Hogyan a szalagtár works legjobb bemutatott példa szerint.</span><span class="sxs-lookup"><span data-stu-id="2a74e-234">How this library works is best demonstrated by an example.</span></span>

<span data-ttu-id="2a74e-235">Belső hello **szerializáló** hello mappájában [azure iot-sdk--c tárház](https://github.com/Azure/azure-iot-sdk-c), van egy **minták** nevű mappát, amely tartalmazza az alkalmazás **simplesample \_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-235">Inside hello **serializer** folder in hello [azure-iot-sdk-c repository](https://github.com/Azure/azure-iot-sdk-c), is a **samples** folder that contains an application called **simplesample\_mqtt**.</span></span> <span data-ttu-id="2a74e-236">Ez a minta hello Windows verzió magában foglalja a Visual Studio megoldás a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a74e-236">hello Windows version of this sample includes hello following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="2a74e-237">Ha megnyitja a projektet a Visual Studio 2017, fogadja el a hello kér tooretarget hello projekt toohello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="2a74e-237">If you open this project in Visual Studio 2017, accept hello prompts tooretarget hello project toohello latest version.</span></span>

<span data-ttu-id="2a74e-238">Csakúgy, mint a korábbi minta hello, ez egy több NuGet-csomagot tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2a74e-238">As with hello previous sample, this one includes several NuGet packages:</span></span>

* <span data-ttu-id="2a74e-239">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="2a74e-239">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="2a74e-240">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="2a74e-240">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="2a74e-241">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="2a74e-241">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="2a74e-242">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="2a74e-242">Microsoft.Azure.IoTHub.Serializer</span></span>
* <span data-ttu-id="2a74e-243">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="2a74e-243">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="2a74e-244">Most láthatta, ezeket a csomagokat hello korábbi minta a legtöbb, de **Microsoft.Azure.IoTHub.Serializer** új.</span><span class="sxs-lookup"><span data-stu-id="2a74e-244">You've seen most of these packages in hello previous sample, but **Microsoft.Azure.IoTHub.Serializer** is new.</span></span> <span data-ttu-id="2a74e-245">Ez a csomag szükséges használata esetén hello **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="2a74e-245">This package is required when you use hello **serializer** library.</span></span>

<span data-ttu-id="2a74e-246">Hello mintaalkalmazás hello végrehajtásának hello található **simplesample\_mqtt.c** fájlt.</span><span class="sxs-lookup"><span data-stu-id="2a74e-246">You can find hello implementation of hello sample application in hello **simplesample\_mqtt.c** file.</span></span>

<span data-ttu-id="2a74e-247">hello alábbiakban ismerteti a minta hello kulcs részei között.</span><span class="sxs-lookup"><span data-stu-id="2a74e-247">hello following sections walk you through hello key parts of this sample.</span></span>

### <a name="initialize-hello-library"></a><span data-ttu-id="2a74e-248">Hello kódtár inicializálása</span><span class="sxs-lookup"><span data-stu-id="2a74e-248">Initialize hello library</span></span>

<span data-ttu-id="2a74e-249">hello használata toostart **szerializáló** könyvtár, hívás hello inicializálási API-kat:</span><span class="sxs-lookup"><span data-stu-id="2a74e-249">toostart working with hello **serializer** library, call hello initialization APIs:</span></span>

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

<span data-ttu-id="2a74e-250">hello hívás toohello **szerializáló\_init** függvény egyszeri hívás és inicializál hello alapul szolgáló könyvtár.</span><span class="sxs-lookup"><span data-stu-id="2a74e-250">hello call toohello **serializer\_init** function is a one-time call and initializes hello underlying library.</span></span> <span data-ttu-id="2a74e-251">Ezután meghívja a hello **IoTHubClient\_inden\_CreateFromConnectionString** függvénynek, amely van hello hello hasonlóan azonos API **IoTHubClient** minta.</span><span class="sxs-lookup"><span data-stu-id="2a74e-251">Then, you call hello **IoTHubClient\_LL\_CreateFromConnectionString** function, which is hello same API as in hello **IoTHubClient** sample.</span></span> <span data-ttu-id="2a74e-252">Ez a hívás beállítja az eszköz kapcsolati karakterlánc (Ez a hívás is, ha úgy dönt, hogy hello protokoll toouse szeretné).</span><span class="sxs-lookup"><span data-stu-id="2a74e-252">This call sets your device connection string (this call is also where you choose hello protocol you want toouse).</span></span> <span data-ttu-id="2a74e-253">Ez a minta hello átvitelhez MQTT használ, de AMQP vagy HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a74e-253">This sample uses MQTT as hello transport, but could use AMQP or HTTP.</span></span>

<span data-ttu-id="2a74e-254">Végezetül hívás hello **létrehozása\_MODELL\_példány** függvény.</span><span class="sxs-lookup"><span data-stu-id="2a74e-254">Finally, call hello **CREATE\_MODEL\_INSTANCE** function.</span></span> <span data-ttu-id="2a74e-255">**WeatherStation** hello névtér hello modell és **ContosoAnemometer** hello modell hello neve van.</span><span class="sxs-lookup"><span data-stu-id="2a74e-255">**WeatherStation** is hello namespace of hello model and **ContosoAnemometer** is hello name of hello model.</span></span> <span data-ttu-id="2a74e-256">Hello modell példány létrehozása után is használhatja az üzenetek toostart küldése és fogadása.</span><span class="sxs-lookup"><span data-stu-id="2a74e-256">Once hello model instance is created, you can use it toostart sending and receiving messages.</span></span> <span data-ttu-id="2a74e-257">Azt azonban fontos toounderstand milyen modell.</span><span class="sxs-lookup"><span data-stu-id="2a74e-257">However, it's important toounderstand what a model is.</span></span>

### <a name="define-hello-model"></a><span data-ttu-id="2a74e-258">Hello modellt</span><span class="sxs-lookup"><span data-stu-id="2a74e-258">Define hello model</span></span>

<span data-ttu-id="2a74e-259">Egy modell a(z) hello **szerializáló** könyvtár határozza meg, amely az eszköz küld tooIoT Hub és hello nevezett üzenetváltással kezdődnek, köszönőüzenetei *műveletek* hello modellezési nyelv, amely fogadja a.</span><span class="sxs-lookup"><span data-stu-id="2a74e-259">A model in hello **serializer** library defines hello messages that your device can send tooIoT Hub and hello messages, called *actions* in hello modeling language, which it can receive.</span></span> <span data-ttu-id="2a74e-260">A modellek, mint hello C makrók használatával meghatározásához **simplesample\_mqtt** mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="2a74e-260">You define a model using a set of C macros as in hello **simplesample\_mqtt** sample application:</span></span>

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

<span data-ttu-id="2a74e-261">Hello **MEGKEZDÉSÉHEZ\_NÉVTÉR** és **END\_NÉVTÉR** mindkét makrók hello névtér hello modell argumentumként igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="2a74e-261">hello **BEGIN\_NAMESPACE** and **END\_NAMESPACE** macros both take hello namespace of hello model as an argument.</span></span> <span data-ttu-id="2a74e-262">Valószínű, hogy semmit, ezek a makrók között-e a modell vagy a modellek és a hello adatstruktúrák hello modellek használó hello meghatározása.</span><span class="sxs-lookup"><span data-stu-id="2a74e-262">It's expected that anything between these macros is hello definition of your model or models, and hello data structures that hello models use.</span></span>

<span data-ttu-id="2a74e-263">Ebben a példában nincs nevű egyetlen modellt **ContosoAnemometer**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-263">In this example, there is a single model called **ContosoAnemometer**.</span></span> <span data-ttu-id="2a74e-264">Ez a modell határozza meg, hogy az eszköz küldhet tooIoT Hub adatok kétféle: **DeviceId** és **szélsebesség**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-264">This model defines two pieces of data that your device can send tooIoT Hub: **DeviceId** and **WindSpeed**.</span></span> <span data-ttu-id="2a74e-265">Az eszköz megkaphatja három műveleteket (üzenet) is definiálja: **TurnFanOn**, **TurnFanOff**, és **SetAirResistance**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-265">It also defines three actions (messages) that your device can receive: **TurnFanOn**, **TurnFanOff**, and **SetAirResistance**.</span></span> <span data-ttu-id="2a74e-266">Minden adatelemre típussal rendelkezik, és minden művelet nevét (és nem kötelező paraméterek).</span><span class="sxs-lookup"><span data-stu-id="2a74e-266">Each data element has a type, and each action has a name (and optionally a set of parameters).</span></span>

<span data-ttu-id="2a74e-267">hello adatok és a hello modellben definiált műveletek határozza meg, hogy toosend üzenetek tooIoT Hub használja, és beállíthatja küldött toomessages toohello eszköz válaszára API felületéhez.</span><span class="sxs-lookup"><span data-stu-id="2a74e-267">hello data and actions defined in hello model define an API surface that you can use toosend messages tooIoT Hub, and respond toomessages sent toohello device.</span></span> <span data-ttu-id="2a74e-268">Ez a modell használatának legjobb értendő egy példán keresztül.</span><span class="sxs-lookup"><span data-stu-id="2a74e-268">Use of this model is best understood through an example.</span></span>

### <a name="send-messages"></a><span data-ttu-id="2a74e-269">Üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="2a74e-269">Send messages</span></span>

<span data-ttu-id="2a74e-270">hello modell hello adatokat küldhet a központ tooIoT határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2a74e-270">hello model defines hello data you can send tooIoT Hub.</span></span> <span data-ttu-id="2a74e-271">Ebben a példában, amely azt jelenti, hogy egy hello hello segítségével meghatározott két adatelemek **WITH_DATA** makró.</span><span class="sxs-lookup"><span data-stu-id="2a74e-271">In this example, that means one of hello two data items defined using hello **WITH_DATA** macro.</span></span> <span data-ttu-id="2a74e-272">Van több lépéseket szükséges toosend **DeviceId** és **szélsebesség** értékek tooan IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2a74e-272">There are several steps required toosend **DeviceId** and **WindSpeed** values tooan IoT hub.</span></span> <span data-ttu-id="2a74e-273">hello először tooset hello adatokat toosend:</span><span class="sxs-lookup"><span data-stu-id="2a74e-273">hello first is tooset hello data you want toosend:</span></span>

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

<span data-ttu-id="2a74e-274">hello korábban meghatározott modell lehetővé teszi tooset hello értékek úgy, hogy tagjai egy **struct**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-274">hello model you defined earlier enables you tooset hello values by setting members of a **struct**.</span></span> <span data-ttu-id="2a74e-275">A következő szerializálni kívánt toosend üdvözlőüzenetére:</span><span class="sxs-lookup"><span data-stu-id="2a74e-275">Next, serialize hello message you want toosend:</span></span>

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed tooserialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

<span data-ttu-id="2a74e-276">Ez a kód rendezi sorba hello eszközről a felhőbe tooa puffer (által hivatkozott **cél**).</span><span class="sxs-lookup"><span data-stu-id="2a74e-276">This code serializes hello device-to-cloud tooa buffer (referenced by **destination**).</span></span> <span data-ttu-id="2a74e-277">hello kódot, majd meghívja hello **sendMessage** toosend hello üzenet tooIoT Központ működéséhez:</span><span class="sxs-lookup"><span data-stu-id="2a74e-277">hello code then invokes hello **sendMessage** function toosend hello message tooIoT Hub:</span></span>

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable toocreate a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed toohand over hello message tooIoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted hello message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


<span data-ttu-id="2a74e-278">második toolast paramétere hello **IoTHubClient\_inden\_SendEventAsync** hivatkozás tooa visszahívási függvény nevezzük, amikor hello adatok elküldése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="2a74e-278">hello second toolast parameter of **IoTHubClient\_LL\_SendEventAsync** is a reference tooa callback function that's called when hello data is successfully sent.</span></span> <span data-ttu-id="2a74e-279">Íme hello visszahívási függvény hello mintában:</span><span class="sxs-lookup"><span data-stu-id="2a74e-279">Here's hello callback function in hello sample:</span></span>

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

<span data-ttu-id="2a74e-280">hello második paramétere mutató toouser kontextusban; azonos kapott túl hello**IoTHubClient\_inden\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-280">hello second parameter is a pointer toouser context; hello same pointer passed too**IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="2a74e-281">Ebben az esetben hello környezetben egy egyszerű számlálót, de bármilyen lehet.</span><span class="sxs-lookup"><span data-stu-id="2a74e-281">In this case, hello context is a simple counter, but it can be anything you want.</span></span>

<span data-ttu-id="2a74e-282">Ez minden toosending eszközről a felhőbe üzenetek van.</span><span class="sxs-lookup"><span data-stu-id="2a74e-282">That's all there is toosending device-to-cloud messages.</span></span> <span data-ttu-id="2a74e-283">hello csak a bal oldali toocover dolog van hogyan tooreceive üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="2a74e-283">hello only thing left toocover is how tooreceive messages.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="2a74e-284">Hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="2a74e-284">Receive messages</span></span>

<span data-ttu-id="2a74e-285">Egy üzenet fogadását működik, hasonlóképpen toohello üzenetek működéséhez a hello **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2a74e-285">Receiving a message works similarly toohello way messages work in hello **IoTHubClient** library.</span></span> <span data-ttu-id="2a74e-286">Először regisztrálnia üzenet visszahívási függvény:</span><span class="sxs-lookup"><span data-stu-id="2a74e-286">First, you register a message callback function:</span></span>

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable tooIoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

<span data-ttu-id="2a74e-287">Ezután ír hello visszahívási függvény, amelyet ha egy üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2a74e-287">Then, you write hello callback function that's invoked when a message is received:</span></span>

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
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

<span data-ttu-id="2a74e-288">Ez a kód bolierplate--azt rendelkezik hello azonos bármely, a megoldás.</span><span class="sxs-lookup"><span data-stu-id="2a74e-288">This code is boilerplate -- it's hello same for any solution.</span></span> <span data-ttu-id="2a74e-289">Ez a funkció hello üzenetet kap, és gondoskodik összegzésére, valamint megfelelő funkciót toohello hello hívás túl**EXECUTE\_parancs**.</span><span class="sxs-lookup"><span data-stu-id="2a74e-289">This function receives hello message and takes care of routing it toohello appropriate function through hello call too**EXECUTE\_COMMAND**.</span></span> <span data-ttu-id="2a74e-290">hello függvény hívása ezen a ponton a modell hello műveletek hello meghatározása függ.</span><span class="sxs-lookup"><span data-stu-id="2a74e-290">hello function called at this point depends on hello definition of hello actions in your model.</span></span>

<span data-ttu-id="2a74e-291">A modell műveletet ad meg, amikor Ön szükséges tooimplement nevezzük, amikor az eszköz megkapja a megfelelő üdvözlőüzenetére függvényt.</span><span class="sxs-lookup"><span data-stu-id="2a74e-291">When you define an action in your model, you're required tooimplement a function that's called when your device receives hello corresponding message.</span></span> <span data-ttu-id="2a74e-292">Ha például a modell határozza meg a műveletet:</span><span class="sxs-lookup"><span data-stu-id="2a74e-292">For example, if your model defines this action:</span></span>

```c
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="2a74e-293">Adja meg a függvény a következő aláírást:</span><span class="sxs-lookup"><span data-stu-id="2a74e-293">Define a function with this signature:</span></span>

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="2a74e-294">Vegye figyelembe, hogyan hello hello függvény neve megegyezik hello hello művelet hello modellben és, hogy hello hello függvény paraméterei egyeznek-e a hello hello a művelethez megadott paraméterek közül.</span><span class="sxs-lookup"><span data-stu-id="2a74e-294">Note how hello name of hello function matches hello name of hello action in hello model and that hello parameters of hello function match hello parameters specified for hello action.</span></span> <span data-ttu-id="2a74e-295">hello első paraméter megadása mindig kötelező, és a modell mutató toohello példányát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2a74e-295">hello first parameter is always required and contains a pointer toohello instance of your model.</span></span>

<span data-ttu-id="2a74e-296">Hello eszköz az aláírásnak megfelelő üzenetet kap, hello megfelelő függvény neve.</span><span class="sxs-lookup"><span data-stu-id="2a74e-296">When hello device receives a message that matches this signature, hello corresponding function is called.</span></span> <span data-ttu-id="2a74e-297">Ezért vezérelt tooinclude hello bolierplate kóddal a rendelkező **IoTHubMessage**, üzenetek fogadására csak egy egyszerű függvény a modellben megadott műveleteket meghatározása.</span><span class="sxs-lookup"><span data-stu-id="2a74e-297">Therefore, aside from having tooinclude hello boilerplate code from **IoTHubMessage**, receiving messages is just a matter of defining a simple function for each action defined in your model.</span></span>

### <a name="uninitialize-hello-library"></a><span data-ttu-id="2a74e-298">Meghívná hello könyvtár</span><span class="sxs-lookup"><span data-stu-id="2a74e-298">Uninitialize hello library</span></span>

<span data-ttu-id="2a74e-299">Amikor elkészült adatokat küldő, és fogadja az üzeneteket, hello IoT könyvtár is meghívná:</span><span class="sxs-lookup"><span data-stu-id="2a74e-299">When you're done sending data and receiving messages, you can uninitialize hello IoT library:</span></span>

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

<span data-ttu-id="2a74e-300">Mindegyik funkcióhoz három hello a fentiekben ismertetett három inicializálási funkciók igazodik.</span><span class="sxs-lookup"><span data-stu-id="2a74e-300">Each of these three functions aligns with hello three initialization functions described previously.</span></span> <span data-ttu-id="2a74e-301">Ezen API-k hívása biztosítja, hogy a korábban kiosztott erőforrásokat ingyenes.</span><span class="sxs-lookup"><span data-stu-id="2a74e-301">Calling these APIs ensures that you free previously allocated resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a74e-302">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a74e-302">Next Steps</span></span>

<span data-ttu-id="2a74e-303">Ebben a cikkben szereplő hello hello hello tárak használatának alapjaival **C-hez készült SDK Azure IoT-eszközök**. Az rendelkezik külön elegendő információt toounderstand hello SDK, az architektúra, és hogyan tooget hello használata Windows minták tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2a74e-303">This article covered hello basics of using hello libraries in hello **Azure IoT device SDK for C**. It provided you with enough information toounderstand what's included in hello SDK, its architecture, and how tooget started working with hello Windows samples.</span></span> <span data-ttu-id="2a74e-304">hello a következő cikk alapján, amely ismerteti, hogy továbbra is hello SDK hello leírása [további: hello IoTHubClient library](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="2a74e-304">hello next article continues hello description of hello SDK by explaining [more about hello IoTHubClient library](iot-hub-device-sdk-c-iothubclient.md).</span></span>

<span data-ttu-id="2a74e-305">az IoT-központ fejlesztésével kapcsolatos további toolearn lásd: hello [Azure IoT SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="2a74e-305">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="2a74e-306">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="2a74e-306">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="2a74e-307">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="2a74e-307">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
