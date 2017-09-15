#### <a name="to-stop-and-start-a-virtual-device"></a><span data-ttu-id="fde81-101">Virtuális eszköz leállítása és indítása</span><span class="sxs-lookup"><span data-stu-id="fde81-101">To stop and start a virtual device</span></span>
<span data-ttu-id="fde81-102">Egy virtuális eszköz leállításához kattintson a nevére, majd kattintson a **Leállítás** elemre.</span><span class="sxs-lookup"><span data-stu-id="fde81-102">To stop a virtual device, click its name, and then click **Shutdown**.</span></span> <span data-ttu-id="fde81-103">A virtuális eszköz leállítása közben az eszköz állapota **Leállítás folyamatban**.</span><span class="sxs-lookup"><span data-stu-id="fde81-103">While the virtual device is shutting down, its status is **Stopping**.</span></span> <span data-ttu-id="fde81-104">A virtuális eszköz leállítás utáni állapota **Leállítva**.</span><span class="sxs-lookup"><span data-stu-id="fde81-104">After the virtual device is stopped, its status is **Stopped**.</span></span>

<span data-ttu-id="fde81-105">Virtuális eszközt az alábbi parancsmagok segítségével állíthatja le és indíthatja újra.</span><span class="sxs-lookup"><span data-stu-id="fde81-105">Use the following cmdlets to stop and start a virtual device.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-virtual-device"></a><span data-ttu-id="fde81-106">Virtuális eszköz újraindítása</span><span class="sxs-lookup"><span data-stu-id="fde81-106">To restart a virtual device</span></span>
<span data-ttu-id="fde81-107">Ha egy virtuális eszközt futás közben szeretne újraindítani, kattintson a nevére, majd az **Újraindítás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="fde81-107">When a virtual device is running and you want to restart it, click its name, and then click **Restart**.</span></span> <span data-ttu-id="fde81-108">A virtuális eszköz újraindítása közben az eszköz állapota **Újraindítás folyamatban**.</span><span class="sxs-lookup"><span data-stu-id="fde81-108">While the virtual device is restarting, its status is **Restarting**.</span></span> <span data-ttu-id="fde81-109">Ha a virtuális eszköz használatra kész, az eszköz állapota **Fut**.</span><span class="sxs-lookup"><span data-stu-id="fde81-109">When the virtual device is ready for you to use, its status is **Running**.</span></span>

<span data-ttu-id="fde81-110">Az alábbi parancsmag segítségével indítsa újra a virtuális eszközt.</span><span class="sxs-lookup"><span data-stu-id="fde81-110">Use the following cmdlet to restart a virtual device.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

