#### <a name="toostop-and-start-a-cloud-appliance"></a><span data-ttu-id="f7225-101">toostop és a felhő készülék start</span><span class="sxs-lookup"><span data-stu-id="f7225-101">toostop and start a cloud appliance</span></span>

1. <span data-ttu-id="f7225-102">egy felhőalapú készülék toostop nyissa meg a felhő alapplatformjaként VM toohello.</span><span class="sxs-lookup"><span data-stu-id="f7225-102">toostop a cloud appliance, go toohello VM for your cloud appliance.</span></span>
    <span data-ttu-id="f7225-103">![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span><span class="sxs-lookup"><span data-stu-id="f7225-103">![StorSimple Cloud Appliance Virtual Machine](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span></span>

2. <span data-ttu-id="f7225-104">Hello parancssávon kattintson **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="f7225-104">From hello command bar, click **Stop**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. <span data-ttu-id="f7225-106">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="f7225-106">When prompted for confirmation, click **Yes**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. <span data-ttu-id="f7225-108">A leállított virtuális gépeket a rendszer felszabadítja.</span><span class="sxs-lookup"><span data-stu-id="f7225-108">When you stop a VM, it gets deallocated.</span></span> <span data-ttu-id="f7225-109">Hello felhő készülék leáll, miközben állapotú-e **Deallocating**.</span><span class="sxs-lookup"><span data-stu-id="f7225-109">While hello cloud appliance is stopping, its status is **Deallocating**.</span></span> <span data-ttu-id="f7225-110">Hello felhő készülék leállítása után állapotú-e **leállítva (felszabadítva)**.</span><span class="sxs-lookup"><span data-stu-id="f7225-110">After hello cloud appliance is stopped, its status is **Stopped (deallocated)**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. <span data-ttu-id="f7225-112">Amikor egy virtuális gép le van állítva, kattintson a **Start** (gomb használhatóvá váljon) toostart hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f7225-112">Once a VM is stopped, click **Start** (button becomes available) toostart hello VM.</span></span> <span data-ttu-id="f7225-113">Miután hello felhő készülék elindult, állapotú-e **elindítva**.</span><span class="sxs-lookup"><span data-stu-id="f7225-113">After hello cloud appliance has started up, its status is **Started**.</span></span>

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

<span data-ttu-id="f7225-115">A következő parancsmagok toostop hello használja, és indítsa el a felhő alapplatformjaként.</span><span class="sxs-lookup"><span data-stu-id="f7225-115">Use hello following cmdlets toostop and start a cloud appliance.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-cloud-appliance"></a><span data-ttu-id="f7225-116">a felhő készülék toorestart</span><span class="sxs-lookup"><span data-stu-id="f7225-116">toorestart a cloud appliance</span></span>

<span data-ttu-id="f7225-117">egy felhőalapú készülék toorestart nyissa meg a felhő alapplatformjaként VM toohello.</span><span class="sxs-lookup"><span data-stu-id="f7225-117">toorestart a cloud appliance, go toohello VM for your cloud appliance.</span></span> <span data-ttu-id="f7225-118">Hello parancssávon kattintson **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="f7225-118">From hello command bar, click **Restart**.</span></span> <span data-ttu-id="f7225-119">Amikor a rendszer kéri, erősítse meg a hello újraindítás.</span><span class="sxs-lookup"><span data-stu-id="f7225-119">When prompted, confirm hello restart.</span></span> <span data-ttu-id="f7225-120">Hello felhő készüléknek meg toouse készen áll, amikor állapotú-e **futtató**.</span><span class="sxs-lookup"><span data-stu-id="f7225-120">When hello cloud appliance is ready for you toouse, its status is **Running**.</span></span>

![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

<span data-ttu-id="f7225-122">A következő parancsmag toorestart egy felhőalapú készülék hello használata.</span><span class="sxs-lookup"><span data-stu-id="f7225-122">Use hello following cmdlet toorestart a cloud appliance.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

