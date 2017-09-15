> [!div class="op_single_selector"]
> * [<span data-ttu-id="1f5a3-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="1f5a3-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [<span data-ttu-id="1f5a3-102">C#/node.js</span><span class="sxs-lookup"><span data-stu-id="1f5a3-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [<span data-ttu-id="1f5a3-103">C#</span><span class="sxs-lookup"><span data-stu-id="1f5a3-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a><span data-ttu-id="1f5a3-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="1f5a3-104">Introduction</span></span>

<span data-ttu-id="1f5a3-105">A [Ismerkedés az IoT Hub eszköz twins][lnk-twin-tutorial], megtudta, hogyan állítható be az eszköz metaadatait a megoldás háttér használata *címkék*, egy eszköz alkalmazásból eszköz feltételek jelentés használatával *tulajdonságok jelentett*, és lekérdezheti az SQL-szerű nyelv használatával adatokat.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-105">In [Get started with IoT Hub device twins][lnk-twin-tutorial], you learned how to set device metadata from your solution back end using *tags*, report device conditions from a device app using *reported properties*, and query this information using a SQL-like language.</span></span>

<span data-ttu-id="1f5a3-106">Ebben az oktatóanyagban megtanulhatja, hogyan használható a a két eszköz *tulajdonságok szükség* együtt *tulajdonságok jelentett*, és így távolról konfigurálhat az eszközön futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-106">In this tutorial, you will learn how to use the the device twin's *desired properties* along with *reported properties*, to remotely configure device apps.</span></span> <span data-ttu-id="1f5a3-107">Pontosabban Ez az oktatóanyag bemutatja, hogyan egy eszköz iker jelentett és kívánt tulajdonságokkal engedélyezze a többlépéses konfigurálása egy alkalmazást, és adja meg a válnak láthatóvá, a megoldás háttérrendszeréhez, ez a művelet állapotát az összes eszközön.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-107">More specifically, this tutorial shows how a device twin's reported and desired properties enable a multi-step configuration of a device application, and provide the visibility to the solution back end of the status of this operation across all devices.</span></span> <span data-ttu-id="1f5a3-108">Az eszköz-konfigurációk a szerepkör további információhoz található [IoT-központ az eszközkezelés áttekintése][lnk-dm-overview].</span><span class="sxs-lookup"><span data-stu-id="1f5a3-108">You can find more information regarding the role of device configurations in [Overview of device management with IoT Hub][lnk-dm-overview].</span></span>

<span data-ttu-id="1f5a3-109">Magas szinten eszköz twins használata lehetővé teszi, hogy a megoldás háttérrendszeréhez, adja meg a kívánt konfigurációs parancsok küldése helyett a kezelt eszközök.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-109">At a high level, using device twins enables the solution back end to specify the desired configuration for the managed devices, instead of sending specific commands.</span></span> <span data-ttu-id="1f5a3-110">Ez az eszköz feladata a legjobb módszer frissíti a konfigurációját (nagyon fontos, ahol eszközre feltételek azonnal a parancsok végrehajtására hatással IoT forgatókönyvekben) beállítása helyezi a megoldás háttérrendszere folyamatosan jelentéskészítés közben az aktuális állapot és a lehetséges hibaállapotok, a frissítési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-110">This puts the device in charge of setting up the best way to update its configuration (very important in IoT scenarios where specific device conditions affect the ability to immediately carry out specific commands), while continually reporting to the solution back end the current state and potential error conditions of the update process.</span></span> <span data-ttu-id="1f5a3-111">Ez a minta nem műszeres felügyeleti eszközöket, a nagy, mert lehetővé teszi, hogy a megoldás háttérrendszeréhez, hogy a teljes látható-e a konfigurációs folyamat állapotát az összes eszközön.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-111">This pattern is instrumental to the management of large sets of devices, as it enables the solution back end to have full visibility of the state of the configuration process across all devices.</span></span>

> [!NOTE]
> <span data-ttu-id="1f5a3-112">Olyan esetekben, ahol vezérelt eszközök több interaktív módon (egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása), érdemes lehet [módszerek közvetlen][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="1f5a3-112">In scenarios where devices are controlled in a more interactive fashion (turn on a fan from a user-controlled app), consider using [direct methods][lnk-methods].</span></span>
> 
> 

<span data-ttu-id="1f5a3-113">Ebben az oktatóanyagban a megoldás háttérrendszeréhez a céleszközön telemetriai konfigurációját módosítja, és emiatt az adott, az eszköz alkalmazás a többlépéses folyamatot követi egy konfigurációs frissítés (például a számítógép újraindítására szoftver modul, amely ezt az oktatóanyag szimulálja egyszerű késéssel).</span><span class="sxs-lookup"><span data-stu-id="1f5a3-113">In this tutorial, the solution back end changes the telemetry configuration of a target device and, as a result of that, the device app follows a multi-step process to apply a configuration update (for example, requiring a software module restart, which this tutorial simulates with a simple delay).</span></span>

<span data-ttu-id="1f5a3-114">A megoldás háttérrendszeréhez tárolja a konfiguráció a két eszköz kívánt tulajdonságok az alábbi módon:</span><span class="sxs-lookup"><span data-stu-id="1f5a3-114">The solution back end stores the configuration in the device twin's desired properties in the following way:</span></span>

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [!NOTE]
> <span data-ttu-id="1f5a3-115">Konfigurációk lehetnek összetett objektumra, mert általában hozzárendeli egyedi azonosítók (kivonatok vagy [GUID][lnk-guid]) egyszerűbbé teheti az összehasonlítást.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-115">Since configurations can be complex objects, they are usually assigned unique ids (hashes or [GUIDs][lnk-guid]) to simplify their comparisons.</span></span>
> 
> 

<span data-ttu-id="1f5a3-116">Az eszköz alkalmazás jelent a kívánt tulajdonságot tükrözés aktuális konfigurációja **telemetryConfig** jelentett tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="1f5a3-116">The device app reports its current configuration mirroring the desired property **telemetryConfig** in the reported properties:</span></span>

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

<span data-ttu-id="1f5a3-117">Megjegyzés: hogyan a jelentett **telemetryConfig** további tulajdonsága **állapot**, a konfiguráció frissítési folyamat állapotának jelentésére használt.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-117">Note how the reported **telemetryConfig** has an additional property **status**, used to report the state of the configuration update process.</span></span>

<span data-ttu-id="1f5a3-118">Amikor egy új szükségeskonfiguráció érkezik, az eszköz alkalmazás egy függőben lévő konfigurációs adatok módosításával jelentések:</span><span class="sxs-lookup"><span data-stu-id="1f5a3-118">When a new desired configuration is received, the device app reports a pending configuration by changing the information:</span></span>

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

<span data-ttu-id="1f5a3-119">Ezt követően egy későbbi időpontban, az eszköz alkalmazás jelentést a sikeres vagy sikertelen volt-e ezt a műveletet a fenti tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-119">Then, at some later time, the device app will report the success or failure of this operation by updating the above property.</span></span>
<span data-ttu-id="1f5a3-120">Vegye figyelembe, hogy a megoldás háttérrendszeréhez Mitől képes, tetszőleges időpontban, a konfigurációs folyamat állapotának lekérdezése az eszközön.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-120">Note how the solution back end is able, at any time, to query the status of the configuration process across all the devices.</span></span>

<span data-ttu-id="1f5a3-121">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="1f5a3-121">This tutorial shows you how to:</span></span>

* <span data-ttu-id="1f5a3-122">Létrehoz egy szimulált eszköz alkalmazást, amely konfigurációs frissítések kap a megoldás háttérrendszeréhez, és több frissítések jelentések *tulajdonságok jelentett* a konfigurációban folyamatot nem lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-122">Create a simulated device app that receives configuration updates from the solution back end, and reports multiple updates as *reported properties* on the configuration update process.</span></span>
* <span data-ttu-id="1f5a3-123">Hozzon létre egy háttér-alkalmazást, amely frissíti az eszköz kívánt beállításait, és ezután lekérdezi a konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="1f5a3-123">Create a back-end app that updates the desired configuration of a device, and then queries the configuration update process.</span></span>

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
