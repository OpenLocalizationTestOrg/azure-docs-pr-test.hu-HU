## <a name="view-device-telemetry-in-hello-dashboard"></a><span data-ttu-id="214a3-101">Nézet telemetriát hello irányítópult</span><span class="sxs-lookup"><span data-stu-id="214a3-101">View device telemetry in hello dashboard</span></span>
<span data-ttu-id="214a3-102">a távoli figyelési megoldás lehetővé teszi, hogy Ön tooview hello telemetriai az eszközök elküldik tooIoT Hub hello irányítópult hello.</span><span class="sxs-lookup"><span data-stu-id="214a3-102">hello dashboard in hello remote monitoring solution enables you tooview hello telemetry your devices send tooIoT Hub.</span></span>

1. <span data-ttu-id="214a3-103">A böngészőben, visszatérési toohello távoli figyelési megoldást irányítópulton kattintson **eszközök** a hello bal oldali panelen toonavigate toohello **eszközök listában**.</span><span class="sxs-lookup"><span data-stu-id="214a3-103">In your browser, return toohello remote monitoring solution dashboard, click **Devices** in hello left-hand panel toonavigate toohello **Devices list**.</span></span>
2. <span data-ttu-id="214a3-104">A hello **eszközök listában**, láthatja, hogy van-e az eszköz állapotának hello **futtató**.</span><span class="sxs-lookup"><span data-stu-id="214a3-104">In hello **Devices list**, you should see that hello status of your device is **Running**.</span></span> <span data-ttu-id="214a3-105">Ha nem, kattintson a **eszköz engedélyezése** a hello **eszközadatok** panel.</span><span class="sxs-lookup"><span data-stu-id="214a3-105">If not, click **Enable Device** in hello **Device Details** panel.</span></span>
   
    ![Eszközállapot megtekintése][18]
3. <span data-ttu-id="214a3-107">Kattintson a **irányítópult** tooreturn toohello irányítópultot, válassza ki az eszköz hello **eszköz tooView** legördülő tooview a telemetriai adatok.</span><span class="sxs-lookup"><span data-stu-id="214a3-107">Click **Dashboard** tooreturn toohello dashboard, select your device in hello **Device tooView** drop-down tooview its telemetry.</span></span> <span data-ttu-id="214a3-108">hello telemetriai hello mintaalkalmazást a belső hőmérséklet 50 egységek, a külső hőmérséklet 55 mértékegységét és a páratartalom 50 egység.</span><span class="sxs-lookup"><span data-stu-id="214a3-108">hello telemetry from hello sample application is 50 units for internal temperature, 55 units for external temperature, and 50 units for humidity.</span></span>
   
    ![Eszköztelemetria megtekintése][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a><span data-ttu-id="214a3-110">Metódus meghívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="214a3-110">Invoke a method on your device</span></span>
<span data-ttu-id="214a3-111">hello irányítópult hello távoli figyelési megoldás lehetővé teszi tooinvoke módszerek a eszközökön az IoT-központ használatával.</span><span class="sxs-lookup"><span data-stu-id="214a3-111">hello dashboard in hello remote monitoring solution enables you tooinvoke methods on your devices through IoT Hub.</span></span> <span data-ttu-id="214a3-112">Például a távoli felügyeleti megoldás hello hívhat meg a metódus toosimulate eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="214a3-112">For example, in hello remote monitoring solution you can invoke a method toosimulate rebooting a device.</span></span>

1. <span data-ttu-id="214a3-113">Hello távoli figyelési megoldást irányítópulton kattintson **eszközök** a hello bal oldali panelen toonavigate toohello **eszközök listában**.</span><span class="sxs-lookup"><span data-stu-id="214a3-113">In hello remote monitoring solution dashboard, click **Devices** in hello left-hand panel toonavigate toohello **Devices list**.</span></span>
2. <span data-ttu-id="214a3-114">Kattintson a **Eszközazonosító** az eszközt hello **eszközök listában**.</span><span class="sxs-lookup"><span data-stu-id="214a3-114">Click **Device ID** for your device in hello **Devices list**.</span></span>
3. <span data-ttu-id="214a3-115">A hello **eszközadatok** panelen, kattintson a **módszerek**.</span><span class="sxs-lookup"><span data-stu-id="214a3-115">In hello **Device details** panel, click **Methods**.</span></span>
   
    ![Eszközmetódusok][13]
4. <span data-ttu-id="214a3-117">A hello **metódus** legördülő listából válassza **InitiateFirmwareUpdate**, majd a **FWPACKAGEURI** meg egy üres URL-címet.</span><span class="sxs-lookup"><span data-stu-id="214a3-117">In hello **Method** drop-down, select **InitiateFirmwareUpdate**, and then in **FWPACKAGEURI** enter a dummy URL.</span></span> <span data-ttu-id="214a3-118">Kattintson a **metódus meghívása** toocall hello metódus hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="214a3-118">Click **Invoke Method** toocall hello method on hello device.</span></span>
   
    ![Eszközmetódus meghívása][14]
   

5. <span data-ttu-id="214a3-120">Megjelenik egy üzenet, a kód fut, amikor az eszköz hello hello metódus hello konzolon.</span><span class="sxs-lookup"><span data-stu-id="214a3-120">You see a message in hello console running your device code when hello device handles hello method.</span></span> <span data-ttu-id="214a3-121">hello metódus hello eredményei toohello előzmények hello megoldás portal jelentek meg:</span><span class="sxs-lookup"><span data-stu-id="214a3-121">hello results of hello method are added toohello history in hello solution portal:</span></span>

    ![Metódus előzményeinek megtekintése][img-method-history]

## <a name="next-steps"></a><span data-ttu-id="214a3-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="214a3-123">Next steps</span></span>
<span data-ttu-id="214a3-124">hello cikk [testreszabása, előre konfigurált megoldások] [ lnk-customize] néhány bővítheti ezt a mintát módját ismerteti.</span><span class="sxs-lookup"><span data-stu-id="214a3-124">hello article [Customizing preconfigured solutions][lnk-customize] describes some ways you can extend this sample.</span></span> <span data-ttu-id="214a3-125">A lehetséges bővítmények közé tartozik a valódi érzékelők használata és további parancsok implementálása.</span><span class="sxs-lookup"><span data-stu-id="214a3-125">Possible extensions include using real sensors and implementing additional commands.</span></span>

<span data-ttu-id="214a3-126">További tudnivalók hello [hello azureiotsuite.com hely engedélyeinek][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="214a3-126">You can learn more about hello [permissions on hello azureiotsuite.com site][lnk-permissions].</span></span>

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
