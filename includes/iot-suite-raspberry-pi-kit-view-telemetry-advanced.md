## <a name="view-hello-telemetry"></a><span data-ttu-id="ddb4c-101">Nézet hello telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="ddb4c-101">View hello telemetry</span></span>

<span data-ttu-id="ddb4c-102">hello málna Pi most küld telemetriai toohello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-102">hello Raspberry Pi is now sending telemetry toohello remote monitoring solution.</span></span> <span data-ttu-id="ddb4c-103">Hello telemetriai hello megoldás irányítópultján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-103">You can view hello telemetry on hello solution dashboard.</span></span> <span data-ttu-id="ddb4c-104">Üzenetek tooyour málna Pi hello megoldás irányítópultról is küldhet.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-104">You can also send messages tooyour Raspberry Pi from hello solution dashboard.</span></span>

- <span data-ttu-id="ddb4c-105">Keresse meg a toohello megoldás irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-105">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="ddb4c-106">Válassza ki az eszköz hello **eszköz tooView** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-106">Select your device in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="ddb4c-107">a hello hello telemetriai málna Pi hello irányítópulton jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-107">hello telemetry from hello Raspberry Pi displays on hello dashboard.</span></span>

![Hello málna Pi a telemetriai adatok megjelenítéséhez][img-telemetry-display]

## <a name="initiate-hello-firmware-update"></a><span data-ttu-id="ddb4c-109">Kezdeményezési hello belső vezérlőprogram frissítése</span><span class="sxs-lookup"><span data-stu-id="ddb4c-109">Initiate hello firmware update</span></span>

<span data-ttu-id="ddb4c-110">hello belső vezérlőprogram frissítési folyamat tölti le, és hello málna Pi hello eszköz ügyfélalkalmazás frissített verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-110">hello firmware update process downloads and installs an updated version of hello device client application on hello Raspberry Pi.</span></span> <span data-ttu-id="ddb4c-111">Hello belső vezérlőprogram frissítési folyamattal kapcsolatos további információkért lásd: hello belső vezérlőprogram frissítési mintát hello leírása [IoT-központ az eszközkezelés áttekintése][lnk-update-pattern].</span><span class="sxs-lookup"><span data-stu-id="ddb4c-111">For more information about hello firmware update process, see hello description of hello firmware update pattern in [Overview of device management with IoT Hub][lnk-update-pattern].</span></span>

<span data-ttu-id="ddb4c-112">Hello belső vezérlőprogram frissítési folyamat kezdeményezése egy metódus hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-112">You initiate hello firmware update process by invoking a method on hello device.</span></span> <span data-ttu-id="ddb4c-113">Ez a metódus aszinkron, és adja vissza, amint hello frissítési folyamat megkezdése.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-113">This method is asynchronous, and returns as soon as hello update process begins.</span></span> <span data-ttu-id="ddb4c-114">hello eszköz által használt tulajdonságok toonotify hello megoldás jelentett kapcsolatos hello frissítés hello előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-114">hello device uses reported properties toonotify hello solution about hello progress of hello update.</span></span>

<span data-ttu-id="ddb4c-115">Módszerek a málna Pi hello megoldás irányítópultról a hívja meg.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-115">You invoke methods on your Raspberry Pi from hello solution dashboard.</span></span> <span data-ttu-id="ddb4c-116">Hello málna Pi először csatlakozik toohello távoli figyelési megoldást igényelnek, ha elküldi az támogatja-e hello módszerekkel kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="ddb4c-116">When hello Raspberry Pi first connects toohello remote monitoring solution, it sends information about hello methods it supports.</span></span> 

[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md