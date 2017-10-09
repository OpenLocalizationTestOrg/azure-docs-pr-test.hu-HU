## <a name="view-hello-telemetry"></a><span data-ttu-id="8286e-101">Nézet hello telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="8286e-101">View hello telemetry</span></span>

<span data-ttu-id="8286e-102">hello málna Pi most küld telemetriai toohello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="8286e-102">hello Raspberry Pi is now sending telemetry toohello remote monitoring solution.</span></span> <span data-ttu-id="8286e-103">Hello telemetriai hello megoldás irányítópultján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="8286e-103">You can view hello telemetry on hello solution dashboard.</span></span> <span data-ttu-id="8286e-104">Üzenetek tooyour málna Pi hello megoldás irányítópultról is küldhet.</span><span class="sxs-lookup"><span data-stu-id="8286e-104">You can also send messages tooyour Raspberry Pi from hello solution dashboard.</span></span>

- <span data-ttu-id="8286e-105">Keresse meg a toohello megoldás irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="8286e-105">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="8286e-106">Válassza ki az eszköz hello **eszköz tooView** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="8286e-106">Select your device in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="8286e-107">a hello hello telemetriai málna Pi hello irányítópulton jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8286e-107">hello telemetry from hello Raspberry Pi displays on hello dashboard.</span></span>

![Hello málna Pi a telemetriai adatok megjelenítéséhez][img-telemetry-display]

## <a name="act-on-hello-device"></a><span data-ttu-id="8286e-109">Hello eszközön működésre</span><span class="sxs-lookup"><span data-stu-id="8286e-109">Act on hello device</span></span>

<span data-ttu-id="8286e-110">Hello megoldás irányítópultról módszerek hívhatók meg a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="8286e-110">From hello solution dashboard, you can invoke methods on your Raspberry Pi.</span></span> <span data-ttu-id="8286e-111">Hello málna Pi csatlakozáskor toohello távoli figyelési megoldást igényelnek, elküldi az támogatja-e hello módszerekkel kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="8286e-111">When hello Raspberry Pi connects toohello remote monitoring solution, it sends information about hello methods it supports.</span></span>

- <span data-ttu-id="8286e-112">Hello megoldás irányítópulton kattintson **eszközök** toovisit hello **eszközök** lap.</span><span class="sxs-lookup"><span data-stu-id="8286e-112">In hello solution dashboard, click **Devices** toovisit hello **Devices** page.</span></span> <span data-ttu-id="8286e-113">Válassza ki a málna Pi hello **eszközlista**.</span><span class="sxs-lookup"><span data-stu-id="8286e-113">Select your Raspberry Pi in hello **Device List**.</span></span> <span data-ttu-id="8286e-114">Válassza a **módszerek**:</span><span class="sxs-lookup"><span data-stu-id="8286e-114">Then choose **Methods**:</span></span>

    ![Az irányítópult eszközök][img-list-devices]

- <span data-ttu-id="8286e-116">A hello **metódus meghívása** lapon, válassza ki **LightBlink** a hello **metódus** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="8286e-116">On hello **Invoke Method** page, choose **LightBlink** in hello **Method** dropdown.</span></span>

- <span data-ttu-id="8286e-117">Válasszon **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="8286e-117">Choose **InvokeMethod**.</span></span> <span data-ttu-id="8286e-118">hello szimulátor hello konzolon egy üzenetet jelenít meg hello málna Pi.</span><span class="sxs-lookup"><span data-stu-id="8286e-118">hello simulator prints a message in hello console on hello Raspberry Pi.</span></span> <span data-ttu-id="8286e-119">hello málna Pi hello alkalmazás küld egy visszaigazoló hátsó toohello megoldás irányítópultja:</span><span class="sxs-lookup"><span data-stu-id="8286e-119">hello app on hello Raspberry Pi sends an acknowledgment back toohello solution dashboard:</span></span>

    ![Módszer előzmények megjelenítése][img-method-history]

- <span data-ttu-id="8286e-121">Megváltoztathatja az hello LED be- és kikapcsolását hello segítségével **ChangeLightStatus** metódust egy **LightStatusValue** túl beállítása**1** a a vagy **0** a ki.</span><span class="sxs-lookup"><span data-stu-id="8286e-121">You can switch hello LED on and off using hello **ChangeLightStatus** method with a **LightStatusValue** set too**1** for on or **0** for off.</span></span>

> [!WARNING]
> <span data-ttu-id="8286e-122">Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="8286e-122">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="8286e-123">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="8286e-123">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="8286e-124">Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="8286e-124">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md