> [!div class="op_single_selector"]
> * [<span data-ttu-id="5a5bf-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="5a5bf-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [<span data-ttu-id="5a5bf-102">C#/node.js</span><span class="sxs-lookup"><span data-stu-id="5a5bf-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [<span data-ttu-id="5a5bf-103">C#</span><span class="sxs-lookup"><span data-stu-id="5a5bf-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a><span data-ttu-id="5a5bf-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5a5bf-104">Introduction</span></span>

<span data-ttu-id="5a5bf-105">A [Ismerkedés az IoT Hub eszköz twins][lnk-twin-tutorial], megtudta, hogyan tooset eszköz metaadatait a megoldás háttérrendszere a befejező használata *címkék*, jelentést eszköz feltételek egy eszköz alkalmazásból használatával *tulajdonságok jelentett*, és lekérdezheti az SQL-szerű nyelv használatával adatokat.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-105">In [Get started with IoT Hub device twins][lnk-twin-tutorial], you learned how tooset device metadata from your solution back end using *tags*, report device conditions from a device app using *reported properties*, and query this information using a SQL-like language.</span></span>

<span data-ttu-id="5a5bf-106">Ebből az oktatóanyagból megtudhatja, hogyan toouse hello hello eszköz iker *szükséges tulajdonságok* együtt *tulajdonságok jelentett*, tooremotely eszköz alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-106">In this tutorial, you will learn how toouse hello hello device twin's *desired properties* along with *reported properties*, tooremotely configure device apps.</span></span> <span data-ttu-id="5a5bf-107">Pontosabban Ez az oktatóanyag bemutatja, hogyan egy eszköz iker jelentett és kívánt tulajdonságok engedélyezése egy alkalmazást többlépéses konfigurálása, és hello látható toohello megoldás háttérrendszerének művelet hello állapotáról biztosít minden eszköz.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-107">More specifically, this tutorial shows how a device twin's reported and desired properties enable a multi-step configuration of a device application, and provide hello visibility toohello solution back end of hello status of this operation across all devices.</span></span> <span data-ttu-id="5a5bf-108">Eszközkonfigurációk a hello szerepkör kapcsolatos további információkat is megtalálhatja [IoT-központ az eszközkezelés áttekintése][lnk-dm-overview].</span><span class="sxs-lookup"><span data-stu-id="5a5bf-108">You can find more information regarding hello role of device configurations in [Overview of device management with IoT Hub][lnk-dm-overview].</span></span>

<span data-ttu-id="5a5bf-109">Magas szinten az eszköz twins használata lehetővé hello megoldás háttér toospecify hello szükségeskonfiguráció hello felügyelt eszközök esetén a konkrét parancsok küldése helyett.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-109">At a high level, using device twins enables hello solution back end toospecify hello desired configuration for hello managed devices, instead of sending specific commands.</span></span> <span data-ttu-id="5a5bf-110">Ez helyezi hello eszköz beállítása hello legjobb módja tooupdate konfigurációjában (nagyon fontos, IoT forgatókönyvekben, ahol adott eszközhöz feltételek hatással hello képességét tooimmediately végeznek konkrét parancsok) feladata folyamatosan reporting toohello közben megoldás háttérrendszere hello aktuális állapot és a lehetséges hibaállapotok hello frissítési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-110">This puts hello device in charge of setting up hello best way tooupdate its configuration (very important in IoT scenarios where specific device conditions affect hello ability tooimmediately carry out specific commands), while continually reporting toohello solution back end hello current state and potential error conditions of hello update process.</span></span> <span data-ttu-id="5a5bf-111">Ebben a mintában műszeres toohello felügyeleti eszközöket, a nagy megegyezik lehetővé teszi, hogy hello megoldás háttér toohave teljes látható-e hello konfigurációs folyamat hello állapotát az összes eszközön.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-111">This pattern is instrumental toohello management of large sets of devices, as it enables hello solution back end toohave full visibility of hello state of hello configuration process across all devices.</span></span>

> [!NOTE]
> <span data-ttu-id="5a5bf-112">Olyan esetekben, ahol vezérelt eszközök több interaktív módon (egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása), érdemes lehet [módszerek közvetlen][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="5a5bf-112">In scenarios where devices are controlled in a more interactive fashion (turn on a fan from a user-controlled app), consider using [direct methods][lnk-methods].</span></span>
> 
> 

<span data-ttu-id="5a5bf-113">Ebben az oktatóanyagban hello megoldás háttér módosítások hello telemetriai konfigurációs a céleszközön, és, hogy a hello eszközalkalmazás miatt egy folyamat tooapply konfiguráció követi (például a számítógép újraindítására szoftver modul, amely ezt frissítése az oktatóanyag szimulálja egyszerű késéssel).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-113">In this tutorial, hello solution back end changes hello telemetry configuration of a target device and, as a result of that, hello device app follows a multi-step process tooapply a configuration update (for example, requiring a software module restart, which this tutorial simulates with a simple delay).</span></span>

<span data-ttu-id="5a5bf-114">hello megoldás háttérrendszerének tárol hello eszköz iker kívánt tulajdonságokkal a következő módon hello hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-114">hello solution back end stores hello configuration in hello device twin's desired properties in hello following way:</span></span>

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of hello configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [!NOTE]
> <span data-ttu-id="5a5bf-115">Konfigurációk lehetnek összetett objektumra, mert általában hozzárendeli egyedi azonosítók (kivonatok vagy [GUID][lnk-guid]) toosimplify az összehasonlítást.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-115">Since configurations can be complex objects, they are usually assigned unique ids (hashes or [GUIDs][lnk-guid]) toosimplify their comparisons.</span></span>
> 
> 

<span data-ttu-id="5a5bf-116">hello eszközalkalmazás jelentések szükséges hello tulajdonság tükrözés aktuális konfigurációja **telemetryConfig** hello a jelentett tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-116">hello device app reports its current configuration mirroring hello desired property **telemetryConfig** in hello reported properties:</span></span>

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

<span data-ttu-id="5a5bf-117">Megjegyzés: a hogyan hello **telemetryConfig** további tulajdonsága **állapot**, használt tooreport hello hello konfigurációs frissítési folyamat állapotát.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-117">Note how hello reported **telemetryConfig** has an additional property **status**, used tooreport hello state of hello configuration update process.</span></span>

<span data-ttu-id="5a5bf-118">Amikor egy új szükségeskonfiguráció érkezik, hello eszközalkalmazás egy függőben lévő konfigurációs hello információk módosításával jelentések:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-118">When a new desired configuration is received, hello device app reports a pending configuration by changing hello information:</span></span>

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of hello pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

<span data-ttu-id="5a5bf-119">Ezt követően egy későbbi időpontban hello eszközalkalmazás jelentést hello sikeres vagy sikertelen volt a művelet hello fent tulajdonság frissítése.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-119">Then, at some later time, hello device app will report hello success or failure of this operation by updating hello above property.</span></span>
<span data-ttu-id="5a5bf-120">Vegye figyelembe, hogyan hello megoldás háttérrendszerének képes, bármikor hello konfigurációs folyamat összes hello eszközön tooquery hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-120">Note how hello solution back end is able, at any time, tooquery hello status of hello configuration process across all hello devices.</span></span>

<span data-ttu-id="5a5bf-121">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-121">This tutorial shows you how to:</span></span>

* <span data-ttu-id="5a5bf-122">Létrehoz egy szimulált eszköz alkalmazást, amely konfigurációs frissítések kap hello megoldás háttérrendszeréhez, és több frissítések jelentések *tulajdonságok jelentett* hello konfiguráció folyamatot nem lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-122">Create a simulated device app that receives configuration updates from hello solution back end, and reports multiple updates as *reported properties* on hello configuration update process.</span></span>
* <span data-ttu-id="5a5bf-123">Létrehoz egy háttér-alkalmazást, hogy a frissítések hello szükségeskonfiguráció egy eszköz, és majd lekérdezések hello konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-123">Create a back-end app that updates hello desired configuration of a device, and then queries hello configuration update process.</span></span>

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
