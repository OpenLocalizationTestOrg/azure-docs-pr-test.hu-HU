## <a name="view-the-telemetry"></a><span data-ttu-id="2279b-101">A telemetriai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="2279b-101">View the telemetry</span></span>

<span data-ttu-id="2279b-102">A málna Pi telemetriai most küld a távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="2279b-102">The Raspberry Pi is now sending telemetry to the remote monitoring solution.</span></span> <span data-ttu-id="2279b-103">A telemetriai adatokat a megoldás irányítópultján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="2279b-103">You can view the telemetry on the solution dashboard.</span></span> <span data-ttu-id="2279b-104">A málna Pi a megoldás irányítópultról is küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="2279b-104">You can also send messages to your Raspberry Pi from the solution dashboard.</span></span>

- <span data-ttu-id="2279b-105">Nyissa meg a megoldás irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="2279b-105">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="2279b-106">Válassza ki az eszköz a **nézet eszköz** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="2279b-106">Select your device in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="2279b-107">A málna Pi a telemetriai adatok megjelenítése az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="2279b-107">The telemetry from the Raspberry Pi displays on the dashboard.</span></span>

![A málna Pi a telemetriai adatok megjelenítéséhez][img-telemetry-display]

## <a name="act-on-the-device"></a><span data-ttu-id="2279b-109">Az eszközön működésre</span><span class="sxs-lookup"><span data-stu-id="2279b-109">Act on the device</span></span>

<span data-ttu-id="2279b-110">A megoldás irányítópultról módszerek hívhatók meg a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="2279b-110">From the solution dashboard, you can invoke methods on your Raspberry Pi.</span></span> <span data-ttu-id="2279b-111">A málna Pi csatlakozik a távoli felügyeleti megoldás, ha az támogatja-e a módszerekkel kapcsolatos információk küld.</span><span class="sxs-lookup"><span data-stu-id="2279b-111">When the Raspberry Pi connects to the remote monitoring solution, it sends information about the methods it supports.</span></span>

- <span data-ttu-id="2279b-112">A megoldás irányítópulton kattintson **eszközök** és látogasson el a **eszközök** lap.</span><span class="sxs-lookup"><span data-stu-id="2279b-112">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="2279b-113">Válassza ki a Raspberry Pi a a **eszközlista**.</span><span class="sxs-lookup"><span data-stu-id="2279b-113">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="2279b-114">Válassza a **módszerek**:</span><span class="sxs-lookup"><span data-stu-id="2279b-114">Then choose **Methods**:</span></span>

    ![Az irányítópult eszközök][img-list-devices]

- <span data-ttu-id="2279b-116">Az a **metódus meghívása** lapon, válassza ki **LightBlink** a a **metódus** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="2279b-116">On the **Invoke Method** page, choose **LightBlink** in the **Method** dropdown.</span></span>

- <span data-ttu-id="2279b-117">Válasszon **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="2279b-117">Choose **InvokeMethod**.</span></span> <span data-ttu-id="2279b-118">A Pi málna villanás több alkalommal csatlakozik a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="2279b-118">The LED connected to the Raspberry Pi flashes several times.</span></span> <span data-ttu-id="2279b-119">Az alkalmazás a málna Pi nyugtázás visszaküldi a megoldás irányítópultja:</span><span class="sxs-lookup"><span data-stu-id="2279b-119">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard:</span></span>

    ![Módszer előzmények megjelenítése][img-method-history]

- <span data-ttu-id="2279b-121">Megváltoztathatja a LED be és ki használja a **ChangeLightStatus** metódust egy **LightStatusValue** beállítása **1** a a vagy **0** a ki.</span><span class="sxs-lookup"><span data-stu-id="2279b-121">You can switch the LED on and off using the **ChangeLightStatus** method with a **LightStatusValue** set to **1** for on or **0** for off.</span></span>

> [!WARNING]
> <span data-ttu-id="2279b-122">Ha nem adja meg a távoli figyelési megoldást igényelnek fut az Azure-fiókjával, a futtatásakor a kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="2279b-122">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="2279b-123">További információ a felhasználás csökkentése a távoli felügyeleti megoldás futtatása közben: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="2279b-123">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="2279b-124">Ha befejezte, használja az előkonfigurált megoldás törlése az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="2279b-124">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md