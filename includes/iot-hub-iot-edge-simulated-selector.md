> [!div class="op_single_selector"]
> * [<span data-ttu-id="9ad0e-101">Linux</span><span class="sxs-lookup"><span data-stu-id="9ad0e-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [<span data-ttu-id="9ad0e-102">Windows</span><span class="sxs-lookup"><span data-stu-id="9ad0e-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

<span data-ttu-id="9ad0e-103">Ez a forgatókönyv a hello [szimulált eszköz felhő feltöltése minta] bemutatja, hogyan toouse [Azure IoT peremhálózati] [ lnk-sdk] toosend eszközről a felhőbe telemetriai tooIoT Hub a szimulált azok az eszközök.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-103">This walkthrough of hello [Simulated Device Cloud Upload sample] shows you how toouse [Azure IoT Edge][lnk-sdk] toosend device-to-cloud telemetry tooIoT Hub from simulated devices.</span></span>

<span data-ttu-id="9ad0e-104">A bemutató tartalma:</span><span class="sxs-lookup"><span data-stu-id="9ad0e-104">This walkthrough covers:</span></span>

* <span data-ttu-id="9ad0e-105">**Architektúra**: hello architekturális információ [szimulált eszköz felhő feltöltése minta].</span><span class="sxs-lookup"><span data-stu-id="9ad0e-105">**Architecture**: architectural information about hello [Simulated Device Cloud Upload sample].</span></span>
* <span data-ttu-id="9ad0e-106">**Létrehozása és futtatása**: hello lépéseket szükséges toobuild és futtatási hello minta.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-106">**Build and run**: hello steps required toobuild and run hello sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="9ad0e-107">Architektúra</span><span class="sxs-lookup"><span data-stu-id="9ad0e-107">Architecture</span></span>

<span data-ttu-id="9ad0e-108">Hello [szimulált eszköz felhő feltöltése minta] bemutatja, hogyan toocreate egy átjáró által küldött telemetriai adatokat a szimulált eszköz tooan IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-108">hello [Simulated Device Cloud Upload sample] shows how toocreate a gateway that sends telemetry from simulated devices tooan IoT hub.</span></span> <span data-ttu-id="9ad0e-109">Egy eszköz nem lehet képes tooconnect közvetlenül tooIoT Hub mert hello eszköz:</span><span class="sxs-lookup"><span data-stu-id="9ad0e-109">A device may not be able tooconnect directly tooIoT Hub because hello device:</span></span>

* <span data-ttu-id="9ad0e-110">Az IoT-központ által értelmezhető kommunikációs protokoll nem használja.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-110">Does not use a communications protocol understood by IoT Hub.</span></span>
* <span data-ttu-id="9ad0e-111">Nincs elég intelligens tooremember hello hozzárendelt identitás tooit IoT hubból.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-111">Is not smart enough tooremember hello identity assigned tooit by IoT Hub.</span></span>

<span data-ttu-id="9ad0e-112">Az IoT-átjáró a következő módokon hello e problémák megoldását:</span><span class="sxs-lookup"><span data-stu-id="9ad0e-112">An IoT Edge gateway can solve these problems in hello following ways:</span></span>

* <span data-ttu-id="9ad0e-113">hello átjáró hello eszköz által használt hello protokoll megértette, eszköz-felhő telemetriai kap hello eszköz, és továbbítja azokat üzenetek tooIoT Hub érthető hello IoT hub protokoll használatát.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-113">hello gateway understands hello protocol used by hello device, receives device-to-cloud telemetry from hello device, and forwards those messages tooIoT Hub using a protocol understood by hello IoT hub.</span></span>

* <span data-ttu-id="9ad0e-114">hello átjáró IoT-központ identitások toodevices le, és amely proxyként funkcionál, ha egy eszköz üzenetek tooIoT Hub küld.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-114">hello gateway maps IoT Hub identities toodevices and acts as a proxy when a device sends messages tooIoT Hub.</span></span>

<span data-ttu-id="9ad0e-115">a következő diagram hello hello hello minta fő összetevőit, beleértve hello IoT peremhálózati modulok jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="9ad0e-115">hello following diagram shows hello main components of hello sample, including hello IoT Edge modules:</span></span>

![][1]

<span data-ttu-id="9ad0e-116">hello modulok nem továbbítja az üzeneteket közvetlenül tooeach más.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-116">hello modules do not pass messages directly tooeach other.</span></span> <span data-ttu-id="9ad0e-117">hello modulok üzenetek tooan belső broker letölti a hello üzenetek toohello más modulok egy előfizetés alapján történő közzétételére.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-117">hello modules publish messages tooan internal broker that delivers hello messages toohello other modules using a subscription mechanism.</span></span> <span data-ttu-id="9ad0e-118">További információkért lásd: [Ismerkedés az Azure IoT peremhálózati][lnk-gw-getstarted].</span><span class="sxs-lookup"><span data-stu-id="9ad0e-118">For more information, see [Get started with Azure IoT Edge][lnk-gw-getstarted].</span></span>

### <a name="protocol-ingestion-module"></a><span data-ttu-id="9ad0e-119">Protokollfeldolgozási modul</span><span class="sxs-lookup"><span data-stu-id="9ad0e-119">Protocol ingestion module</span></span>

<span data-ttu-id="9ad0e-120">Ez a modul kiindulási pontjaként fogadó adatokat az eszközökről, hello átjárón keresztül, és hello felhőbe hello.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-120">This module is hello starting point for receiving data from devices, through hello gateway, and into hello cloud.</span></span> <span data-ttu-id="9ad0e-121">Hello minta hello modul:</span><span class="sxs-lookup"><span data-stu-id="9ad0e-121">In hello sample, hello module:</span></span>

1. <span data-ttu-id="9ad0e-122">Szimulált hőmérséklet adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-122">Creates simulated temperature data.</span></span> <span data-ttu-id="9ad0e-123">Ha a fizikai eszközök használatához hello modul olvassa be az adatokat fizikai eszközöket.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-123">If you use physical devices, hello module reads data from those physical devices.</span></span>
1. <span data-ttu-id="9ad0e-124">Létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-124">Creates a message.</span></span>
1. <span data-ttu-id="9ad0e-125">Szimulált hello hőmérséklet adatok helyezi hello üzenet tartalma.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-125">Places hello simulated temperature data into hello message content.</span></span>
1. <span data-ttu-id="9ad0e-126">Hozzáad egy tulajdonság hamis MAC-cím toohello üzenetet.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-126">Adds a property with a fake MAC address toohello message.</span></span>
1. <span data-ttu-id="9ad0e-127">Lehetővé teszi a hello üzenet elérhető toohello tovább modul hello láncában található.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-127">Makes hello message available toohello next module in hello chain.</span></span>

<span data-ttu-id="9ad0e-128">hello modul nevű **protokoll X adatfeldolgozást** hello az előző ábrán neve **szimulált eszköz** hello forráskód.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-128">hello module called **Protocol X ingestion** in hello previous diagram is called **Simulated device** in hello source code.</span></span>

### <a name="mac-lt-gt-iot-hub-id-module"></a><span data-ttu-id="9ad0e-129">MAC &lt;-&gt; IoT Hub-azonosítómodul</span><span class="sxs-lookup"><span data-stu-id="9ad0e-129">MAC &lt;-&gt; IoT Hub ID module</span></span>

<span data-ttu-id="9ad0e-130">Ez a modul egy Mac-cím tulajdonsággal rendelkező üzenetek keres.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-130">This module scans for messages that have a Mac address property.</span></span> <span data-ttu-id="9ad0e-131">Hello mintában hello protokoll adatfeldolgozást modul hozzáadása hello MAC-cím tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-131">In hello sample, hello protocol ingestion module adds hello MAC address property.</span></span> <span data-ttu-id="9ad0e-132">Hello modul azt észlelte, hogy a tulajdonságot, ha hozzáadja az IoT Hub eszköz fő toohello üzenetet egy másik tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-132">If hello module finds such a property, it adds another property with an IoT Hub device key toohello message.</span></span> <span data-ttu-id="9ad0e-133">hello modul hello üzenet elérhető toohello tovább modul majd teszi hello láncában található.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-133">hello module then makes hello message available toohello next module in hello chain.</span></span>

<span data-ttu-id="9ad0e-134">hello fejlesztői állít be egy MAC-címek és az IoT-központ identitások tooassociate szimulált hello eszközök IoT Hub eszköz identitások közötti leképezést.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-134">hello developer sets up a mapping between MAC addresses and IoT Hub identities tooassociate hello simulated devices with IoT Hub device identities.</span></span> <span data-ttu-id="9ad0e-135">hello fejlesztői hello leképezési hello modul konfigurációjának részeként manuálisan hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-135">hello developer adds hello mapping manually as part of hello module configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="9ad0e-136">Ez a minta egy MAC-címet használ egyedi eszközazonosítóként, és azt egy IoT Hub-eszközidentitáshoz kapcsolja.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-136">This sample uses a MAC address as a unique device identifier and correlates it with an IoT Hub device identity.</span></span> <span data-ttu-id="9ad0e-137">Írhat azonban saját, eltérő egyedi azonosítót használó modult is.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-137">However, you can write your own module that uses a different unique identifier.</span></span> <span data-ttu-id="9ad0e-138">Például előfordulhat, hogy az eszközök egyedi sorozatszámokat vagy hello telemetriai adatok tartalmazhatják a beágyazott eszköz egyedi neve.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-138">For example, your devices may have unique serial numbers or hello telemetry data may include a unique embedded device name.</span></span>

### <a name="iot-hub-communication-module"></a><span data-ttu-id="9ad0e-139">IoT Hub-kommunikációs modul</span><span class="sxs-lookup"><span data-stu-id="9ad0e-139">IoT Hub communication module</span></span>

<span data-ttu-id="9ad0e-140">Ez a modul az IoT-központ üzenetek hello előző modul által hozzárendelt eszköz kulcstulajdonság vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-140">This module takes messages with an IoT Hub device key property that was assigned by hello previous module.</span></span> <span data-ttu-id="9ad0e-141">hello modul hello tartalom tooIoT központ használatával a HTTP protokoll hello üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-141">hello module sends hello message content tooIoT Hub using hello HTTP protocol.</span></span> <span data-ttu-id="9ad0e-142">A HTTP az IoT-központ három protokollok megértettem hello egyik.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-142">HTTP is one of hello three protocols understood by IoT Hub.</span></span>

<span data-ttu-id="9ad0e-143">Az egyes szimulált eszköz kapcsolat létesítése helyett az Ez a modul egyetlen HTTP-kapcsolat megnyitása hello átjáró toohello IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-143">Instead of opening a connection for each simulated device, this module opens a single HTTP connection from hello gateway toohello IoT hub.</span></span> <span data-ttu-id="9ad0e-144">hello modul majd multiplexes minden hello szimulált eszköz kapcsolatokat a kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-144">hello module then multiplexes connections from all hello simulated devices over that connection.</span></span> <span data-ttu-id="9ad0e-145">Ez a megközelítés lehetővé teszi, hogy egy átjáró egyetlen tooconnect számos további eszköz.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-145">This approach enables a single gateway tooconnect many more devices.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="9ad0e-146">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="9ad0e-146">Before you get started</span></span>

<span data-ttu-id="9ad0e-147">Mielőtt hozzáfogna, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="9ad0e-147">Before you get started, you must:</span></span>

* <span data-ttu-id="9ad0e-148">[Létrehoz egy IoT-központot] [ lnk-create-hub] az Azure-előfizetéshez kell hello nevét a hub toocomplete Ez a forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-148">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need hello name of your hub toocomplete this walkthrough.</span></span> <span data-ttu-id="9ad0e-149">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="9ad0e-149">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="9ad0e-150">Két eszközök tooyour IoT hub hozzáadása, és jegyezze fel az azonosítót és az eszköz kulcsok.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-150">Add two devices tooyour IoT hub and make a note of their ids and device keys.</span></span> <span data-ttu-id="9ad0e-151">Használhatja a hello [eszköz explorer] [ lnk-device-explorer] vagy [IOT hubbal-explorer] [ lnk-iothub-explorer] tooadd az eszközök toohello IoT hub hello létrehozott eszközzel előző lépést, valamint a kulcsok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9ad0e-151">You can use hello [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool tooadd your devices toohello IoT hub you created in hello previous step and retrieve their keys.</span></span>

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[szimulált eszköz felhő feltöltése minta]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md