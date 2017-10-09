#### <a name="toostop-and-start-a-virtual-device"></a><span data-ttu-id="79699-101">toostop és a virtuális eszköz indítása</span><span class="sxs-lookup"><span data-stu-id="79699-101">toostop and start a virtual device</span></span>
<span data-ttu-id="79699-102">toostop egy virtuális eszközt, kattintson a nevére, és kattintson a **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="79699-102">toostop a virtual device, click its name, and then click **Shutdown**.</span></span> <span data-ttu-id="79699-103">Hello virtuális eszköz leállítás fázisában van, amíg állapotú-e **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="79699-103">While hello virtual device is shutting down, its status is **Stopping**.</span></span> <span data-ttu-id="79699-104">Hello virtuális eszköz leállítása után állapotú-e **leállítva**.</span><span class="sxs-lookup"><span data-stu-id="79699-104">After hello virtual device is stopped, its status is **Stopped**.</span></span>

<span data-ttu-id="79699-105">A következő parancsmagok toostop hello használja, és indítsa el a virtuális eszköz.</span><span class="sxs-lookup"><span data-stu-id="79699-105">Use hello following cmdlets toostop and start a virtual device.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-virtual-device"></a><span data-ttu-id="79699-106">a virtuális eszköz toorestart</span><span class="sxs-lookup"><span data-stu-id="79699-106">toorestart a virtual device</span></span>
<span data-ttu-id="79699-107">Ha egy virtuális eszközön fut, és azt szeretné, hogy toorestart, kattintson a nevére, és kattintson **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="79699-107">When a virtual device is running and you want toorestart it, click its name, and then click **Restart**.</span></span> <span data-ttu-id="79699-108">Hello virtuális eszköz újraindítása, közben állapotú-e **újraindítása**.</span><span class="sxs-lookup"><span data-stu-id="79699-108">While hello virtual device is restarting, its status is **Restarting**.</span></span> <span data-ttu-id="79699-109">Virtuális eszköz hello meg toouse készen áll, amikor állapotú-e **futtató**.</span><span class="sxs-lookup"><span data-stu-id="79699-109">When hello virtual device is ready for you toouse, its status is **Running**.</span></span>

<span data-ttu-id="79699-110">A következő parancsmag toorestart a virtuális eszköz hello használata.</span><span class="sxs-lookup"><span data-stu-id="79699-110">Use hello following cmdlet toorestart a virtual device.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

