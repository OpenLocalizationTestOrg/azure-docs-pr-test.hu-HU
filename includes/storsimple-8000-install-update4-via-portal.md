<!--author=alkohli last changed: 07/07/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a><span data-ttu-id="a85f6-101">Frissítés telepítése az Azure Portalról</span><span class="sxs-lookup"><span data-stu-id="a85f6-101">To install an update from the Azure portal</span></span>

1. <span data-ttu-id="a85f6-102">A StorSimple szolgáltatás oldalán válassza ki az eszközét.</span><span class="sxs-lookup"><span data-stu-id="a85f6-102">On the StorSimple service page, select your device.</span></span>

    ![Válassza ki az eszköz](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. <span data-ttu-id="a85f6-104">Navigáljon a **eszközbeállítások** > **eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="a85f6-104">Navigate to **Device settings** > **Device updates**.</span></span>

    ![Kattintson az eszköz frissítése](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. <span data-ttu-id="a85f6-106">Megjelenik egy értesítés, ha új frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a85f6-106">A notification appears if new updates are available.</span></span> <span data-ttu-id="a85f6-107">Másik lehetőségként a a **eszközfrissítésekhez** panelen kattintson a **frissítések keresése**.</span><span class="sxs-lookup"><span data-stu-id="a85f6-107">Alternatively, in the **Device updates** blade, click **Scan Updates**.</span></span> <span data-ttu-id="a85f6-108">Létrejön egy feladat, amely megkeresi az elérhető frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="a85f6-108">A job is created to scan for available updates.</span></span> <span data-ttu-id="a85f6-109">A feladat sikeres befejezéséről értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="a85f6-109">You are notified when the job completes successfully.</span></span>

    ![Kattintson az eszköz frissítése](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. <span data-ttu-id="a85f6-111">Azt javasoljuk, hogy mielőtt alkalmazná a frissítést az eszközön, tekintse át a kibocsátási megjegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="a85f6-111">We recommend that you review the release notes before you apply an update on your device.</span></span> <span data-ttu-id="a85f6-112">Frissítések alkalmazásához kattintson **frissítések telepítése**.</span><span class="sxs-lookup"><span data-stu-id="a85f6-112">To apply updates, click **Install updates**.</span></span> <span data-ttu-id="a85f6-113">Az a **erősítse meg a rendszeres frissítések** panelt, tekintse át az előfeltételek telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="a85f6-113">In the **Confirm regular updates** blade, review the prerequisites to complete before you apply updates.</span></span> <span data-ttu-id="a85f6-114">Válassza ki a jelölőnégyzet bejelölésével jelezze, hogy készen áll az eszköz frissítése, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="a85f6-114">Select the checkbox to indicate that you are ready to update the device and then click **Install**.</span></span>

    ![Kattintson az eszköz frissítése](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. <span data-ttu-id="a85f6-116">Megkezdődik az előfeltételek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a85f6-116">A set of prerequisite checks starts.</span></span> <span data-ttu-id="a85f6-117">Ezekbe az ellenőrzésekbe a következők tartoznak:</span><span class="sxs-lookup"><span data-stu-id="a85f6-117">These checks include:</span></span>
   
   * <span data-ttu-id="a85f6-118">**A vezérlő állapotának ellenőrzései** annak ellenőrzéséhez, hogy mindkét eszközvezérlő kifogástalan állapotú és online legyen.</span><span class="sxs-lookup"><span data-stu-id="a85f6-118">**Controller health checks** to verify that both the device controllers are healthy and online.</span></span>
   * <span data-ttu-id="a85f6-119">**A hardverösszetevők állapotának ellenőrzései** annak ellenőrzéséhez, hogy a StorSimple eszközön lévő összes hardverösszetevő kifogástalan állapotú legyen.</span><span class="sxs-lookup"><span data-stu-id="a85f6-119">**Hardware component health checks** to verify that all the hardware components on your StorSimple device are healthy.</span></span>
   * <span data-ttu-id="a85f6-120">**DATA 0 ellenőrzések** annak ellenőrzéséhez, hogy a DATA 0 engedélyezett legyen az eszközön.</span><span class="sxs-lookup"><span data-stu-id="a85f6-120">**DATA 0 checks** to verify that DATA 0 is enabled on your device.</span></span> <span data-ttu-id="a85f6-121">Ha ez az illesztőfelület nem engedélyezett, engedélyeznie kell, majd újra kell próbálkoznia.</span><span class="sxs-lookup"><span data-stu-id="a85f6-121">If this interface is not enabled, you must enable it and then retry.</span></span>

    <span data-ttu-id="a85f6-122">A frissítés letöltése és telepítése csak akkor, ha minden ellenőrzés sikeresen befejeződtek.</span><span class="sxs-lookup"><span data-stu-id="a85f6-122">The update is downloaded and installed only if all the checks are successfully completed.</span></span> <span data-ttu-id="a85f6-123">Értesítést kap, amikor az ellenőrzések folyamatban vannak.</span><span class="sxs-lookup"><span data-stu-id="a85f6-123">You are notified when the checks are in progress.</span></span> <span data-ttu-id="a85f6-124">Ha meghibásodik a prechecks, majd azt fogják ellátni a hiba lehetséges okait.</span><span class="sxs-lookup"><span data-stu-id="a85f6-124">If the prechecks fail, then you will be provided with the reasons for failure.</span></span> <span data-ttu-id="a85f6-125">Hárítsa el ezeket a problémákat, és próbálkozzon újra a művelettel.</span><span class="sxs-lookup"><span data-stu-id="a85f6-125">Address those issues and then retry the operation.</span></span> <span data-ttu-id="a85f6-126">Elképzelhető, hogy kapcsolatba kell lépnie a Microsoft támogatási szolgálatával, ha nem tudja egyedül kezelni ezeket a problémákat.</span><span class="sxs-lookup"><span data-stu-id="a85f6-126">You may need to contact Microsoft Support if you cannot address these issues by yourself.</span></span>

7. <span data-ttu-id="a85f6-127">A prechecks sikeres elvégzése után létrejön egy frissítési feladatot.</span><span class="sxs-lookup"><span data-stu-id="a85f6-127">After the prechecks are successfully completed, an update job is created.</span></span> <span data-ttu-id="a85f6-128">A frissítési feladat sikeres létrehozásáról értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="a85f6-128">You are notified when the update job is successfully created.</span></span>
   
    ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    <span data-ttu-id="a85f6-130">A rendszer ezután alkalmazza a frissítést az eszközre.</span><span class="sxs-lookup"><span data-stu-id="a85f6-130">The update is then applied on your device.</span></span>

9. <span data-ttu-id="a85f6-131">A frissítés elvégzése néhány órát igényel.</span><span class="sxs-lookup"><span data-stu-id="a85f6-131">The update takes a few hours to complete.</span></span> <span data-ttu-id="a85f6-132">Válassza ki a frissítési feladatot, és kattintson a **Részletek** gombra, így bármikor megtekintheti a feladat részleteit.</span><span class="sxs-lookup"><span data-stu-id="a85f6-132">Select the update job and click **Details** to view the details of the job at any time.</span></span>

    ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update8.png)

     <span data-ttu-id="a85f6-134">Ugyanígy figyelheti a frissítési feladat előrehaladásának **eszközbeállítások > feladatok**.</span><span class="sxs-lookup"><span data-stu-id="a85f6-134">You can also monitor the progress of the update job from **Device settings > Jobs**.</span></span> <span data-ttu-id="a85f6-135">Az a **feladatok** panelen láthatja, hogy a frissítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a85f6-135">On the **Jobs** blade, you can see the update progress.</span></span>

     ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. <span data-ttu-id="a85f6-137">A feladat befejezése után nyissa meg a **eszközbeállítások > eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="a85f6-137">After the job is complete, navigate to the **Device settings > Device updates**.</span></span> <span data-ttu-id="a85f6-138">A szoftver verziója most frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="a85f6-138">The software version should now be updated.</span></span>

    ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update9.png)

