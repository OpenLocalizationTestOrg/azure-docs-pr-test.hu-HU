<!--author=alkohli last changed: 01/18/17 -->

#### <a name="to-install-updates-via-the-azure-portal"></a><span data-ttu-id="cb727-101">Frissítések telepítése az Azure Portalon keresztül</span><span class="sxs-lookup"><span data-stu-id="cb727-101">To install updates via the Azure portal</span></span>

1. <span data-ttu-id="cb727-102">Nyissa meg a StorSimple-eszközkezelőt, és válassza az **Eszközök** elemet.</span><span class="sxs-lookup"><span data-stu-id="cb727-102">Go to your StorSimple Device Manager and select **Devices**.</span></span> <span data-ttu-id="cb727-103">A szolgáltatáshoz csatlakozó eszközök listájából válassza ki a frissíteni kívánt eszközt, majd kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="cb727-103">From the list of devices connected to your service, select and click the device you want to update.</span></span> 

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate1m.png) 

2. <span data-ttu-id="cb727-105">A **Beállítások** panelen kattintson az **Eszközfrissítések** elemre.</span><span class="sxs-lookup"><span data-stu-id="cb727-105">In the **Settings** blade, click **Device updates**.</span></span> 

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate2m.png)  

3. <span data-ttu-id="cb727-107">Megjelenik a szoftverfrissítések elérhetőségét jelző üzenet.</span><span class="sxs-lookup"><span data-stu-id="cb727-107">You see a message if the software updates are available.</span></span> <span data-ttu-id="cb727-108">A frissítések kereséséhez a **Vizsgálat** gombot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="cb727-108">To check for updates, you can also click **Scan**.</span></span>

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate3m1.png)

    <span data-ttu-id="cb727-110">A vizsgálat indításáról és sikeres befejezéséről értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="cb727-110">You will be notified when the scan starts and completes successfully.</span></span>

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate5m.png)

4. <span data-ttu-id="cb727-112">Ha elkészült a frissítések vizsgálata, kattintson a **Frissítések letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb727-112">Once the updates are scanned, click **Download updates**.</span></span> 

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate6m.png)

5. <span data-ttu-id="cb727-114">Az **Új frissítések** panel tájékoztatja arról, hogy a frissítések letöltése után meg kell erősítenie a telepítésüket.</span><span class="sxs-lookup"><span data-stu-id="cb727-114">In the **New updates** blade, review the information that after the updates are downloaded, you need to confirm the installation.</span></span> <span data-ttu-id="cb727-115">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb727-115">Click **OK**.</span></span>

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate7m.png)

6. <span data-ttu-id="cb727-117">A feltöltés indításáról és sikeres befejezéséről értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="cb727-117">You are notified when the upload starts and completes successfully.</span></span>

     ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate8m.png)

5. <span data-ttu-id="cb727-119">Az **Eszközfrissítések** panelen kattintson a **Telepítés** elemre.</span><span class="sxs-lookup"><span data-stu-id="cb727-119">In the **Device updates** blade, click **Install**.</span></span>

     ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate11m1.png)   

6. <span data-ttu-id="cb727-121">Az **Új frissítések** panelen a rendszer figyelmezteti, hogy a frissítési folyamat megszakítja az eszköz egyéb folyamatait.</span><span class="sxs-lookup"><span data-stu-id="cb727-121">In the **New updates** blade, you are warned that the update is disruptive.</span></span> <span data-ttu-id="cb727-122">Mivel a virtuális tömb egy egyetlen csomóponttal rendelkező eszköz, ezért az eszköz a frissítést követően újraindul.</span><span class="sxs-lookup"><span data-stu-id="cb727-122">As virtual array is a single node device, the device restarts after it is updated.</span></span> <span data-ttu-id="cb727-123">Ez megszakítja a folyamatban lévő I/O-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="cb727-123">This disrupts any IO in progress.</span></span> <span data-ttu-id="cb727-124">A frissítések telepítéséhez kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb727-124">Click **OK** to install the updates.</span></span> 

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate12m.png) 

7. <span data-ttu-id="cb727-126">A telepítési feladat indításáról értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="cb727-126">You are notified when the install job starts.</span></span> 

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate13m.png)

8.  <span data-ttu-id="cb727-128">A telepítési feladat sikeres befejezése után a telepítés eredményeinek megtekintéséhez kattintson a **Feladat megtekintése** hivatkozásra az **Eszközfrissítések** panelen.</span><span class="sxs-lookup"><span data-stu-id="cb727-128">After the install job completes successfully, click **View Job** link in the **Device updates** blade to monitor the installation.</span></span> 

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate15m1.png)

    <span data-ttu-id="cb727-130">Ezzel továbblép a **Frissítések telepítése** panelre.</span><span class="sxs-lookup"><span data-stu-id="cb727-130">This takes you to the **Install Updates** blade.</span></span> <span data-ttu-id="cb727-131">A feladatról részletes információt itt talál.</span><span class="sxs-lookup"><span data-stu-id="cb727-131">You can view detailed information about the job here.</span></span>

    ![eszköz frissítése](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate16m1.png)

9. <span data-ttu-id="cb727-133">A frissítések sikeres telepítéséről értesítést láthat az **Eszközfrissítések** panelen.</span><span class="sxs-lookup"><span data-stu-id="cb727-133">After the updates are successfully installed, you see a message to this effect in the **Device updates** blade.</span></span> 