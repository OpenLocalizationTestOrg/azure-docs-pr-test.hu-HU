<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a><span data-ttu-id="62939-101">A bekapcsolási az eszköz bekábelezése</span><span class="sxs-lookup"><span data-stu-id="62939-101">To cable your device for power</span></span>
> [!NOTE]
> <span data-ttu-id="62939-102">A StorSimple eszköz mindkét ház redundáns PCMs tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="62939-102">Both enclosures on your StorSimple device include redundant PCMs.</span></span> <span data-ttu-id="62939-103">Az egyes ház a PCMs telepítve legyen, és csatlakozik a különböző áramforrásokból magas rendelkezésre állásának biztosításához.</span><span class="sxs-lookup"><span data-stu-id="62939-103">For each enclosure, the PCMs must be installed and connected to different power sources to ensure high availability.</span></span>
> 
> 

1. <span data-ttu-id="62939-104">Győződjön meg arról, hogy az összes PCMs power kapcsolókkal megfelelő jogkörrel OFF.</span><span class="sxs-lookup"><span data-stu-id="62939-104">Make sure that the power switches on all the PCMs are in the OFF position.</span></span>
2. <span data-ttu-id="62939-105">A elsődleges tárolóeszközön csatlakoztassa a tápkábelek mindkét PCMs.</span><span class="sxs-lookup"><span data-stu-id="62939-105">On the primary enclosure, connect the power cords to both PCMs.</span></span> <span data-ttu-id="62939-106">A tápkábelek alatt a power kábelezési diagramon piros azonosítja.</span><span class="sxs-lookup"><span data-stu-id="62939-106">The power cords are identified in red in the power cabling diagram, below.</span></span>
3. <span data-ttu-id="62939-107">Győződjön meg arról, hogy a két PCMs a elsődleges tárolóeszközön külön áramforrásokból.</span><span class="sxs-lookup"><span data-stu-id="62939-107">Make sure that the two PCMs on the primary enclosure use separate power sources.</span></span>
4. <span data-ttu-id="62939-108">Ahogy az ábra kábelek a power vezeték csatlakoztatni az állvány Áramelosztó egységekből bekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="62939-108">Attach the power cords to the power on the rack distribution units as shown in the power cabling diagram.</span></span>
5. <span data-ttu-id="62939-109">Ismételje meg a 2 – 4. a EBOD ház.</span><span class="sxs-lookup"><span data-stu-id="62939-109">Repeat steps 2 through 4 for the EBOD enclosure.</span></span>
6. <span data-ttu-id="62939-110">Kapcsolja be a EBOD ház által az egyes PCM az ON-pozíció a kikapcsolás tükrözés.</span><span class="sxs-lookup"><span data-stu-id="62939-110">Turn on the EBOD enclosure by flipping the power switch on each PCM to the ON position.</span></span>
7. <span data-ttu-id="62939-111">Győződjön meg arról, hogy a a EBOD ház úgy, hogy a EBOD vezérlő hátulján zöld LED vannak kapcsolja be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="62939-111">Verify that the EBOD enclosure is turned on by checking that the green LEDs on the back of the EBOD controller are turned ON.</span></span>
8. <span data-ttu-id="62939-112">Kapcsolja be az elsődleges ház minden PCM kapcsoló átállításával a ON helyre.</span><span class="sxs-lookup"><span data-stu-id="62939-112">Turn on the primary enclosure by flipping each PCM switch to the ON position.</span></span>
9. <span data-ttu-id="62939-113">Győződjön meg arról, hogy a rendszer a biztosításával, hogy az eszköz tartományvezérlő LED be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="62939-113">Verify that the system is on by ensuring the device controller LEDs have turned ON.</span></span>
10. <span data-ttu-id="62939-114">Ellenőrizze, hogy a kapcsolatot a EBOD vezérlő és az eszköz vezérlő aktív szerint annak ellenőrzése, hogy a négy LED a SAS-port a EBOD vezérlőn melletti zöld.</span><span class="sxs-lookup"><span data-stu-id="62939-114">Make sure that the connection between the EBOD controller and the device controller is active by verifying that the four LEDs next to the SAS port on the EBOD controller are green.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="62939-115">A rendszer magas rendelkezésre állás biztosítása érdekében azt javasoljuk, hogy Ön szigorúan igazodnia kell a energiagazdálkodási séma a következő ábrán is látható kábelek.</span><span class="sxs-lookup"><span data-stu-id="62939-115">To ensure high availability for your system, we recommend that you strictly adhere to the power cabling scheme shown in the following diagram.</span></span>
    > 
    > 
    
    ![Az energiagazdálkodási 4U eszköz bekábelezése](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    <span data-ttu-id="62939-117">**Energiagazdálkodási kábelek**</span><span class="sxs-lookup"><span data-stu-id="62939-117">**Power cabling**</span></span>
    
    | <span data-ttu-id="62939-118">Címke</span><span class="sxs-lookup"><span data-stu-id="62939-118">Label</span></span> | <span data-ttu-id="62939-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="62939-119">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="62939-120">1</span><span class="sxs-lookup"><span data-stu-id="62939-120">1</span></span> |<span data-ttu-id="62939-121">Elsődleges ház</span><span class="sxs-lookup"><span data-stu-id="62939-121">Primary enclosure</span></span> |
    | <span data-ttu-id="62939-122">2</span><span class="sxs-lookup"><span data-stu-id="62939-122">2</span></span> |<span data-ttu-id="62939-123">PCM 0</span><span class="sxs-lookup"><span data-stu-id="62939-123">PCM 0</span></span> |
    | <span data-ttu-id="62939-124">3</span><span class="sxs-lookup"><span data-stu-id="62939-124">3</span></span> |<span data-ttu-id="62939-125">PCM 1</span><span class="sxs-lookup"><span data-stu-id="62939-125">PCM 1</span></span> |
    | <span data-ttu-id="62939-126">4</span><span class="sxs-lookup"><span data-stu-id="62939-126">4</span></span> |<span data-ttu-id="62939-127">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="62939-127">Controller 0</span></span> |
    | <span data-ttu-id="62939-128">5</span><span class="sxs-lookup"><span data-stu-id="62939-128">5</span></span> |<span data-ttu-id="62939-129">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="62939-129">Controller 1</span></span> |
    | <span data-ttu-id="62939-130">6</span><span class="sxs-lookup"><span data-stu-id="62939-130">6</span></span> |<span data-ttu-id="62939-131">EBOD vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="62939-131">EBOD controller 0</span></span> |
    | <span data-ttu-id="62939-132">7</span><span class="sxs-lookup"><span data-stu-id="62939-132">7</span></span> |<span data-ttu-id="62939-133">1. EBOD vezérlő</span><span class="sxs-lookup"><span data-stu-id="62939-133">EBOD controller 1</span></span> |
    | <span data-ttu-id="62939-134">8</span><span class="sxs-lookup"><span data-stu-id="62939-134">8</span></span> |<span data-ttu-id="62939-135">EBOD ház</span><span class="sxs-lookup"><span data-stu-id="62939-135">EBOD enclosure</span></span> |
    | <span data-ttu-id="62939-136">9</span><span class="sxs-lookup"><span data-stu-id="62939-136">9</span></span> |<span data-ttu-id="62939-137">PDU-k</span><span class="sxs-lookup"><span data-stu-id="62939-137">PDUs</span></span> |

