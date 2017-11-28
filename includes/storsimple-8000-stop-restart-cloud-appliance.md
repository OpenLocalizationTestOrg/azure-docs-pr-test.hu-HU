#### <a name="to-stop-and-start-a-cloud-appliance"></a><span data-ttu-id="50ac5-101">Felhőalapú készülék leállítása és indítása</span><span class="sxs-lookup"><span data-stu-id="50ac5-101">To stop and start a cloud appliance</span></span>

1. <span data-ttu-id="50ac5-102">Felhőalapú készülék leállításához lépjen a felhőalapú berendezéshez tartozó virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="50ac5-102">To stop a cloud appliance, go to the VM for your cloud appliance.</span></span>
    <span data-ttu-id="50ac5-103">![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span><span class="sxs-lookup"><span data-stu-id="50ac5-103">![StorSimple Cloud Appliance Virtual Machine](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span></span>

2. <span data-ttu-id="50ac5-104">A parancssávon kattintson a **Leállítás** elemre.</span><span class="sxs-lookup"><span data-stu-id="50ac5-104">From the command bar, click **Stop**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. <span data-ttu-id="50ac5-106">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="50ac5-106">When prompted for confirmation, click **Yes**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. <span data-ttu-id="50ac5-108">A leállított virtuális gépeket a rendszer felszabadítja.</span><span class="sxs-lookup"><span data-stu-id="50ac5-108">When you stop a VM, it gets deallocated.</span></span> <span data-ttu-id="50ac5-109">Amíg a felhőalapú készülék leállítása folyamatban van, az állapota **Felszabadítás**.</span><span class="sxs-lookup"><span data-stu-id="50ac5-109">While the cloud appliance is stopping, its status is **Deallocating**.</span></span> <span data-ttu-id="50ac5-110">A felhőalapú készülék leállítás utáni állapota **Leállítva (felszabadítva)**.</span><span class="sxs-lookup"><span data-stu-id="50ac5-110">After the cloud appliance is stopped, its status is **Stopped (deallocated)**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. <span data-ttu-id="50ac5-112">Ha a virtuális gép leállt, kattintson a **Start** gombra (amikor a gomb elérhetővé válik) a virtuális gép elindításához.</span><span class="sxs-lookup"><span data-stu-id="50ac5-112">Once a VM is stopped, click **Start** (button becomes available) to start the VM.</span></span> <span data-ttu-id="50ac5-113">A felhőalapú készülék elindulás utáni állapota **Elindítva**.</span><span class="sxs-lookup"><span data-stu-id="50ac5-113">After the cloud appliance has started up, its status is **Started**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

<span data-ttu-id="50ac5-115">A felhőalapú készülékeket az alábbi parancsmagok segítségével állíthatja le és indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="50ac5-115">Use the following cmdlets to stop and start a cloud appliance.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-cloud-appliance"></a><span data-ttu-id="50ac5-116">Felhőalapú készülék újraindítása</span><span class="sxs-lookup"><span data-stu-id="50ac5-116">To restart a cloud appliance</span></span>

<span data-ttu-id="50ac5-117">Felhőalapú készülék újraindításához lépjen a felhőalapú berendezéshez tartozó virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="50ac5-117">To restart a cloud appliance, go to the VM for your cloud appliance.</span></span> <span data-ttu-id="50ac5-118">A parancssávon kattintson az **Újraindítás** elemre.</span><span class="sxs-lookup"><span data-stu-id="50ac5-118">From the command bar, click **Restart**.</span></span> <span data-ttu-id="50ac5-119">A rendszer kérésére erősítse meg az újraindítást.</span><span class="sxs-lookup"><span data-stu-id="50ac5-119">When prompted, confirm the restart.</span></span> <span data-ttu-id="50ac5-120">Ha a felhőalapú készülék használatra kész, az eszköz állapota **Fut**.</span><span class="sxs-lookup"><span data-stu-id="50ac5-120">When the cloud appliance is ready for you to use, its status is **Running**.</span></span>

![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

<span data-ttu-id="50ac5-122">A felhőalapú készüléket az alábbi parancsmaggal indíthatja újra.</span><span class="sxs-lookup"><span data-stu-id="50ac5-122">Use the following cmdlet to restart a cloud appliance.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

