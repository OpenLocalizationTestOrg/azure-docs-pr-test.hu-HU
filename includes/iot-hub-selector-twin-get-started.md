> [!div class="op_single_selector"]
> * [<span data-ttu-id="40ae6-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="40ae6-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [<span data-ttu-id="40ae6-102">C#/node.js</span><span class="sxs-lookup"><span data-stu-id="40ae6-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [<span data-ttu-id="40ae6-103">C#</span><span class="sxs-lookup"><span data-stu-id="40ae6-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [<span data-ttu-id="40ae6-104">Java</span><span class="sxs-lookup"><span data-stu-id="40ae6-104">Java</span></span>](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

<span data-ttu-id="40ae6-105">Az ikereszközök JSON-dokumentumok, amelyek az eszközök állapotinformációit (metaadatokat, konfigurációkat és állapotokat) tárolják.</span><span class="sxs-lookup"><span data-stu-id="40ae6-105">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="40ae6-106">Az IoT-központ továbbra is fennáll, egy eszköz iker tooit csatlakozó eszközökről.</span><span class="sxs-lookup"><span data-stu-id="40ae6-106">IoT Hub persists a device twin for each device that connects tooit.</span></span>

<span data-ttu-id="40ae6-107">Az eszköz twins használja:</span><span class="sxs-lookup"><span data-stu-id="40ae6-107">Use device twins to:</span></span>

* <span data-ttu-id="40ae6-108">A megoldás háttérből eszköz metaadatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="40ae6-108">Store device metadata from your solution back end.</span></span>
* <span data-ttu-id="40ae6-109">Például a rendelkezésre álló lehetőségeket, és a feltételek (például hello kapcsolat használt módszer) aktuális állapotadatokat jelentést az eszköz alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="40ae6-109">Report current state information such as available capabilities and conditions (for example, hello connectivity method used) from your device app.</span></span>
* <span data-ttu-id="40ae6-110">Egy eszköz alkalmazás és a háttér-alkalmazások között (például a belső vezérlőprogram és konfigurációja frissítések) hosszan futó munkafolyamatok hello állapotának szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="40ae6-110">Synchronize hello state of long-running workflows (such as firmware and configuration updates) between a device app and a back-end app.</span></span>
* <span data-ttu-id="40ae6-111">Az eszköz metaadatait, konfigurációs vagy az állapot lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="40ae6-111">Query your device metadata, configuration, or state.</span></span>

> [!NOTE]
> <span data-ttu-id="40ae6-112">Eszköz twins úgy tervezték, a szinkronizálás és eszköz-konfigurációk és a kikötések lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="40ae6-112">Device twins are designed for synchronization and for querying device configurations and conditions.</span></span> <span data-ttu-id="40ae6-113">Ha toouse eszköz twins itt található: a további informations [eszköz twins megértéséhez][lnk-twins].</span><span class="sxs-lookup"><span data-stu-id="40ae6-113">More informations on when toouse device twins can be found in [Understand device twins][lnk-twins].</span></span>

<span data-ttu-id="40ae6-114">Eszköz twins az IoT-központ tárolódnak, és tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="40ae6-114">Device twins are stored in an IoT hub and contain:</span></span>

* <span data-ttu-id="40ae6-115">*címkék*, csak hello megoldás háttérrendszerének; által elérhető eszköz metaadatait</span><span class="sxs-lookup"><span data-stu-id="40ae6-115">*tags*, device metadata accessible only by hello solution back end;</span></span>
* <span data-ttu-id="40ae6-116">*szükségeskonfiguráció-tulajdonságok*, hello megoldás által módosítható JSON-objektumok biztonsági vége és megfigyelhető hello eszközalkalmazás; és</span><span class="sxs-lookup"><span data-stu-id="40ae6-116">*desired properties*, JSON objects modifiable by hello solution back end and observable by hello device app; and</span></span>
* <span data-ttu-id="40ae6-117">*Tulajdonságok jelentett*, JSON-objektumok hello eszköz alkalmazás által módosítható és hello megoldás háttérrendszerének által is olvasható.</span><span class="sxs-lookup"><span data-stu-id="40ae6-117">*reported properties*, JSON objects modifiable by hello device app and readable by hello solution back end.</span></span> <span data-ttu-id="40ae6-118">Címkék és a Tulajdonságok tömb nem tartalmazhat, de az objektumok egymásba ágyazható.</span><span class="sxs-lookup"><span data-stu-id="40ae6-118">Tags and properties cannot contain arrays, but objects can be nested.</span></span>

![][img-twin]

<span data-ttu-id="40ae6-119">Emellett hello megoldás háttérrendszerének lekérdezheti az eszköz twins összes hello fenti adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="40ae6-119">Additionally, hello solution back end can query device twins based on all hello above data.</span></span>
<span data-ttu-id="40ae6-120">Tekintse meg a túl[eszköz twins megértéséhez] [ lnk-twins] további információt az eszköz twins és toohello [IoT-központ lekérdezési nyelv] [ lnk-query] referencia a lekérdezésre.</span><span class="sxs-lookup"><span data-stu-id="40ae6-120">Refer too[Understand device twins][lnk-twins] for more information about device twins, and toohello [IoT Hub query language][lnk-query] reference for querying.</span></span>

> [!NOTE]
> <span data-ttu-id="40ae6-121">Ilyenkor eszköz twins elérhetők, csak az eszközök, tooIoT központi csatlakozás hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="40ae6-121">At this time, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="40ae6-122">Tekintse meg a toohello [MQTT támogatási] [ lnk-devguide-mqtt] a cikk útmutatást tooconvert meglévő eszköz alkalmazás toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="40ae6-122">Please refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>

<span data-ttu-id="40ae6-123">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="40ae6-123">This tutorial shows you how to:</span></span>

* <span data-ttu-id="40ae6-124">Hozzon létre egy háttér-alkalmazást, amely *címkék* tooa eszköz iker, és a szimulált eszköz alkalmazást, amely kapcsolatát jelentések csatorna egy *tulajdonság jelentett* a hello eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="40ae6-124">Create a back-end app that adds *tags* tooa device twin, and a simulated device app that reports its connectivity channel as a *reported property* on hello device twin.</span></span>
* <span data-ttu-id="40ae6-125">A háttér-alkalmazás szűrők használata a hello címkék és a korábban létrehozott tulajdonságok eszközök lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="40ae6-125">Query devices from your back-end app using filters on hello tags and properties previously created.</span></span>

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md