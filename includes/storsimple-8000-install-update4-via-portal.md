<!--author=alkohli last changed: 07/07/17-->

#### <a name="tooinstall-an-update-from-hello-azure-portal"></a><span data-ttu-id="1eefa-101">tooinstall frissített hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1eefa-101">tooinstall an update from hello Azure portal</span></span>

1. <span data-ttu-id="1eefa-102">Hello StorSimple szolgáltatás lapon jelölje be az eszközt.</span><span class="sxs-lookup"><span data-stu-id="1eefa-102">On hello StorSimple service page, select your device.</span></span>

    ![Válassza ki az eszköz](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. <span data-ttu-id="1eefa-104">Keresse meg a túl**eszközbeállítások** > **eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="1eefa-104">Navigate too**Device settings** > **Device updates**.</span></span>

    ![Kattintson az eszköz frissítése](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. <span data-ttu-id="1eefa-106">Megjelenik egy értesítés, ha új frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="1eefa-106">A notification appears if new updates are available.</span></span> <span data-ttu-id="1eefa-107">Másik lehetőségként a hello **eszközfrissítésekhez** panelen kattintson a **frissítések keresése**.</span><span class="sxs-lookup"><span data-stu-id="1eefa-107">Alternatively, in hello **Device updates** blade, click **Scan Updates**.</span></span> <span data-ttu-id="1eefa-108">Egy feladat jön létre az elérhető frissítések tooscan.</span><span class="sxs-lookup"><span data-stu-id="1eefa-108">A job is created tooscan for available updates.</span></span> <span data-ttu-id="1eefa-109">Értesítést kap hello feladat sikeresen befejeződik.</span><span class="sxs-lookup"><span data-stu-id="1eefa-109">You are notified when hello job completes successfully.</span></span>

    ![Kattintson az eszköz frissítése](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. <span data-ttu-id="1eefa-111">Javasoljuk, hogy hello kibocsátási megjegyzések tekintse át az eszközön egy frissítés alkalmazása előtt.</span><span class="sxs-lookup"><span data-stu-id="1eefa-111">We recommend that you review hello release notes before you apply an update on your device.</span></span> <span data-ttu-id="1eefa-112">tooapply frissítések **frissítések telepítése**.</span><span class="sxs-lookup"><span data-stu-id="1eefa-112">tooapply updates, click **Install updates**.</span></span> <span data-ttu-id="1eefa-113">A hello **erősítse meg a rendszeres frissítések** panelt, tekintse át hello Előfeltételek toocomplete frissítések alkalmazása előtt.</span><span class="sxs-lookup"><span data-stu-id="1eefa-113">In hello **Confirm regular updates** blade, review hello prerequisites toocomplete before you apply updates.</span></span> <span data-ttu-id="1eefa-114">Válassza ki a hello jelölőnégyzet tooindicate, hogy készen áll a tooupdate hello eszközt, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="1eefa-114">Select hello checkbox tooindicate that you are ready tooupdate hello device and then click **Install**.</span></span>

    ![Kattintson az eszköz frissítése](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. <span data-ttu-id="1eefa-116">Megkezdődik az előfeltételek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1eefa-116">A set of prerequisite checks starts.</span></span> <span data-ttu-id="1eefa-117">Ezekbe az ellenőrzésekbe a következők tartoznak:</span><span class="sxs-lookup"><span data-stu-id="1eefa-117">These checks include:</span></span>
   
   * <span data-ttu-id="1eefa-118">**A tartományvezérlő állapotellenőrzést** , hogy mindkét hello eszközvezérlők tooverify kifogástalan és online.</span><span class="sxs-lookup"><span data-stu-id="1eefa-118">**Controller health checks** tooverify that both hello device controllers are healthy and online.</span></span>
   * <span data-ttu-id="1eefa-119">**Hardver összetevő állapotellenőrzést** , hogy az összes hello hardverösszetevők a StorSimple eszköz tooverify kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="1eefa-119">**Hardware component health checks** tooverify that all hello hardware components on your StorSimple device are healthy.</span></span>
   * <span data-ttu-id="1eefa-120">**DATA 0 ellenőrzi** tooverify, hogy a DATA 0 engedélyezve van az eszközön.</span><span class="sxs-lookup"><span data-stu-id="1eefa-120">**DATA 0 checks** tooverify that DATA 0 is enabled on your device.</span></span> <span data-ttu-id="1eefa-121">Ha ez az illesztőfelület nem engedélyezett, engedélyeznie kell, majd újra kell próbálkoznia.</span><span class="sxs-lookup"><span data-stu-id="1eefa-121">If this interface is not enabled, you must enable it and then retry.</span></span>

    <span data-ttu-id="1eefa-122">hello frissítés letöltése és telepítése csak akkor, ha az hello-ellenőrzés sikeresen befejeződtek.</span><span class="sxs-lookup"><span data-stu-id="1eefa-122">hello update is downloaded and installed only if all hello checks are successfully completed.</span></span> <span data-ttu-id="1eefa-123">Értesítést kap hello ellenőrzése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="1eefa-123">You are notified when hello checks are in progress.</span></span> <span data-ttu-id="1eefa-124">Ha hello prechecks meghibásodik, majd azt fogják ellátni hello a hiba lehetséges okait.</span><span class="sxs-lookup"><span data-stu-id="1eefa-124">If hello prechecks fail, then you will be provided with hello reasons for failure.</span></span> <span data-ttu-id="1eefa-125">Hárítsa el ezeket a problémákat, és próbálkozzon újra a hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="1eefa-125">Address those issues and then retry hello operation.</span></span> <span data-ttu-id="1eefa-126">Ha saját maga által nem megoldja ezeket a problémákat, akkor esetleg toocontact Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="1eefa-126">You may need toocontact Microsoft Support if you cannot address these issues by yourself.</span></span>

7. <span data-ttu-id="1eefa-127">Hello prechecks sikeres elvégzése után létrejön egy frissítési feladatot.</span><span class="sxs-lookup"><span data-stu-id="1eefa-127">After hello prechecks are successfully completed, an update job is created.</span></span> <span data-ttu-id="1eefa-128">Értesítést kap hello frissítési feladat sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="1eefa-128">You are notified when hello update job is successfully created.</span></span>
   
    ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    <span data-ttu-id="1eefa-130">hello frissítés alkalmazva lesz az eszközön.</span><span class="sxs-lookup"><span data-stu-id="1eefa-130">hello update is then applied on your device.</span></span>

9. <span data-ttu-id="1eefa-131">hello frissítés néhány óra toocomplete időbe telik.</span><span class="sxs-lookup"><span data-stu-id="1eefa-131">hello update takes a few hours toocomplete.</span></span> <span data-ttu-id="1eefa-132">Válassza ki a hello feladat frissítése, és kattintson a **részletek** tooview hello bármikor hello feladat részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="1eefa-132">Select hello update job and click **Details** tooview hello details of hello job at any time.</span></span>

    ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update8.png)

     <span data-ttu-id="1eefa-134">Is kísérheti hello hello frissítési feladat **eszközbeállítások > feladatok**.</span><span class="sxs-lookup"><span data-stu-id="1eefa-134">You can also monitor hello progress of hello update job from **Device settings > Jobs**.</span></span> <span data-ttu-id="1eefa-135">A hello **feladatok** panelen látható hello frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="1eefa-135">On hello **Jobs** blade, you can see hello update progress.</span></span>

     ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. <span data-ttu-id="1eefa-137">Hello feladat befejezése után nyissa meg a toohello **eszközbeállítások > eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="1eefa-137">After hello job is complete, navigate toohello **Device settings > Device updates**.</span></span> <span data-ttu-id="1eefa-138">hello szoftver verziója most frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="1eefa-138">hello software version should now be updated.</span></span>

    ![Frissítési feladat létrehozása](./media/storsimple-8000-install-update4-via-portal/update9.png)

