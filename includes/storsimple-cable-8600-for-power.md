<!--author=alkohli last changed: 9/16/15-->


#### <a name="toocable-your-device-for-power"></a><span data-ttu-id="6f91b-101">toocable power az eszköz</span><span class="sxs-lookup"><span data-stu-id="6f91b-101">toocable your device for power</span></span>
> [!NOTE]
> <span data-ttu-id="6f91b-102">A StorSimple eszköz mindkét ház redundáns PCMs tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6f91b-102">Both enclosures on your StorSimple device include redundant PCMs.</span></span> <span data-ttu-id="6f91b-103">Az egyes ház hello PCMs telepíteni kell, és toodifferent power források tooensure magas rendelkezésre állású csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="6f91b-103">For each enclosure, hello PCMs must be installed and connected toodifferent power sources tooensure high availability.</span></span>
> 
> 

1. <span data-ttu-id="6f91b-104">Győződjön meg arról, hogy minden hello PCMs hello power kapcsolókkal hello OFF állásban van-e.</span><span class="sxs-lookup"><span data-stu-id="6f91b-104">Make sure that hello power switches on all hello PCMs are in hello OFF position.</span></span>
2. <span data-ttu-id="6f91b-105">Hello elsődleges ház, a csatlakozás hello power zsinór tooboth PCMs.</span><span class="sxs-lookup"><span data-stu-id="6f91b-105">On hello primary enclosure, connect hello power cords tooboth PCMs.</span></span> <span data-ttu-id="6f91b-106">az alábbi ábrán, kábelek hello power piros hello tápkábelek azonosítja.</span><span class="sxs-lookup"><span data-stu-id="6f91b-106">hello power cords are identified in red in hello power cabling diagram, below.</span></span>
3. <span data-ttu-id="6f91b-107">Győződjön meg arról, hogy a hello elsődleges ház használata külön áramforrásokból két PCMs hello.</span><span class="sxs-lookup"><span data-stu-id="6f91b-107">Make sure that hello two PCMs on hello primary enclosure use separate power sources.</span></span>
4. <span data-ttu-id="6f91b-108">Hello power zsinór toohello bekapcsolás hello állvány Áramelosztó egységekből, ahogy az ábra kábelek hello power csatolni.</span><span class="sxs-lookup"><span data-stu-id="6f91b-108">Attach hello power cords toohello power on hello rack distribution units as shown in hello power cabling diagram.</span></span>
5. <span data-ttu-id="6f91b-109">Hello EBOD ház ismételje meg a 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="6f91b-109">Repeat steps 2 through 4 for hello EBOD enclosure.</span></span>
6. <span data-ttu-id="6f91b-110">Kapcsolja be a hello EBOD ház minden PCM toohello ON el a kikapcsolás hello átállításával.</span><span class="sxs-lookup"><span data-stu-id="6f91b-110">Turn on hello EBOD enclosure by flipping hello power switch on each PCM toohello ON position.</span></span>
7. <span data-ttu-id="6f91b-111">Győződjön meg arról, hogy hello EBOD ház úgy, hogy a zöld hello LED hello hello EBOD vezérlő oldalán a vannak-e kapcsolva a ON be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="6f91b-111">Verify that hello EBOD enclosure is turned on by checking that hello green LEDs on hello back of hello EBOD controller are turned ON.</span></span>
8. <span data-ttu-id="6f91b-112">Kapcsolja be a hello elsődleges ház által tükrözés minden PCM kapcsoló toohello ON el.</span><span class="sxs-lookup"><span data-stu-id="6f91b-112">Turn on hello primary enclosure by flipping each PCM switch toohello ON position.</span></span>
9. <span data-ttu-id="6f91b-113">Győződjön meg arról, hogy hello rendszer a hello eszköz vezérlő LED be van kapcsolva biztosításával.</span><span class="sxs-lookup"><span data-stu-id="6f91b-113">Verify that hello system is on by ensuring hello device controller LEDs have turned ON.</span></span>
10. <span data-ttu-id="6f91b-114">Ellenőrizze, hogy hello kapcsolat hello EBOD vezérlő és hello eszköz vezérlő aktív szerint annak ellenőrzése, hogy hello négy LED következő toohello SAS-portjához hello EBOD vezérlő zöld.</span><span class="sxs-lookup"><span data-stu-id="6f91b-114">Make sure that hello connection between hello EBOD controller and hello device controller is active by verifying that hello four LEDs next toohello SAS port on hello EBOD controller are green.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="6f91b-115">tooensure magas rendelkezésre állás a rendszerhez, ajánlott toohello energiagazdálkodási séma hello a következő ábrán látható kábelek szigorúan igazodik.</span><span class="sxs-lookup"><span data-stu-id="6f91b-115">tooensure high availability for your system, we recommend that you strictly adhere toohello power cabling scheme shown in hello following diagram.</span></span>
    > 
    > 
    
    ![Az energiagazdálkodási 4U eszköz bekábelezése](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    <span data-ttu-id="6f91b-117">**Energiagazdálkodási kábelek**</span><span class="sxs-lookup"><span data-stu-id="6f91b-117">**Power cabling**</span></span>
    
    | <span data-ttu-id="6f91b-118">Címke</span><span class="sxs-lookup"><span data-stu-id="6f91b-118">Label</span></span> | <span data-ttu-id="6f91b-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="6f91b-119">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="6f91b-120">1</span><span class="sxs-lookup"><span data-stu-id="6f91b-120">1</span></span> |<span data-ttu-id="6f91b-121">Elsődleges ház</span><span class="sxs-lookup"><span data-stu-id="6f91b-121">Primary enclosure</span></span> |
    | <span data-ttu-id="6f91b-122">2</span><span class="sxs-lookup"><span data-stu-id="6f91b-122">2</span></span> |<span data-ttu-id="6f91b-123">PCM 0</span><span class="sxs-lookup"><span data-stu-id="6f91b-123">PCM 0</span></span> |
    | <span data-ttu-id="6f91b-124">3</span><span class="sxs-lookup"><span data-stu-id="6f91b-124">3</span></span> |<span data-ttu-id="6f91b-125">PCM 1</span><span class="sxs-lookup"><span data-stu-id="6f91b-125">PCM 1</span></span> |
    | <span data-ttu-id="6f91b-126">4</span><span class="sxs-lookup"><span data-stu-id="6f91b-126">4</span></span> |<span data-ttu-id="6f91b-127">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="6f91b-127">Controller 0</span></span> |
    | <span data-ttu-id="6f91b-128">5</span><span class="sxs-lookup"><span data-stu-id="6f91b-128">5</span></span> |<span data-ttu-id="6f91b-129">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="6f91b-129">Controller 1</span></span> |
    | <span data-ttu-id="6f91b-130">6</span><span class="sxs-lookup"><span data-stu-id="6f91b-130">6</span></span> |<span data-ttu-id="6f91b-131">EBOD vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="6f91b-131">EBOD controller 0</span></span> |
    | <span data-ttu-id="6f91b-132">7</span><span class="sxs-lookup"><span data-stu-id="6f91b-132">7</span></span> |<span data-ttu-id="6f91b-133">1. EBOD vezérlő</span><span class="sxs-lookup"><span data-stu-id="6f91b-133">EBOD controller 1</span></span> |
    | <span data-ttu-id="6f91b-134">8</span><span class="sxs-lookup"><span data-stu-id="6f91b-134">8</span></span> |<span data-ttu-id="6f91b-135">EBOD ház</span><span class="sxs-lookup"><span data-stu-id="6f91b-135">EBOD enclosure</span></span> |
    | <span data-ttu-id="6f91b-136">9</span><span class="sxs-lookup"><span data-stu-id="6f91b-136">9</span></span> |<span data-ttu-id="6f91b-137">PDU-k</span><span class="sxs-lookup"><span data-stu-id="6f91b-137">PDUs</span></span> |

