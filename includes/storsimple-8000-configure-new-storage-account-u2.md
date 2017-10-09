<!--author=alkohli last changed: 01/20/17-->


#### <a name="tooadd-a-storage-account-credential-in-hello-same-azure-subscription-as-hello-storsimple-device-manager-service"></a><span data-ttu-id="65cab-101">tooadd a tárfiók hitelesítő adatokat a hello azonos Azure-előfizetés hello StorSimple Device Manager szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="65cab-101">tooadd a storage account credential in hello same Azure subscription as hello StorSimple Device Manager service</span></span>

1. <span data-ttu-id="65cab-102">Nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="65cab-102">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="65cab-103">A hello **konfigurációs** kattintson **Tárfiók hitelesítő adatainak**.</span><span class="sxs-lookup"><span data-stu-id="65cab-103">In hello **Configuration** section, click **Storage account credentials**.</span></span>

    ![Tárfiók hitelesítő adatai](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct1.png)

2. <span data-ttu-id="65cab-105">A hello **Tárfiók hitelesítő adatainak** panelen kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="65cab-105">On hello **Storage account credentials** blade, click **+ Add**.</span></span>

    ![Tárfiók hitelesítő adatainak hozzáadása](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct2.png)

3. <span data-ttu-id="65cab-107">A hello **adja hozzá a tárolási fiók hitelesítő adatait** panelen hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="65cab-107">In hello **Add a storage account credential** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="65cab-108">Ad hozzá a tárolási fiók hitelesítő adatait, hello azonos Azure-előfizetés szolgáltatásként, győződjön meg arról, hogy **aktuális** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="65cab-108">As you are adding a storage account credential in hello same Azure subscription as your service, ensure that **Current** is selected.</span></span>

    2. <span data-ttu-id="65cab-109">A hello **tárfiók** legördülő listára, válassza ki a meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="65cab-109">From hello **storage account** dropdown list, select an existing storage account.</span></span>

    3. <span data-ttu-id="65cab-110">Hello kijelölve tárfiók alapján, hello **hely** jelenik meg (szürkén jelenik meg, és itt nem módosítható).</span><span class="sxs-lookup"><span data-stu-id="65cab-110">Based on hello storage account selected, hello **location** will be displayed (grayed out and cannot be changed here).</span></span>

    4. <span data-ttu-id="65cab-111">Válassza ki **SSL-mód engedélyezése** toocreate biztonságos csatornát a eszköz- és hello felhő közötti hálózati kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="65cab-111">Select **Enable SSL Mode** toocreate a secure channel for network communication between your device and hello cloud.</span></span> <span data-ttu-id="65cab-112">Csak akkor tiltsa le az **SSL engedélyezése** elemet, ha magánfelhőben tevékenykedik.</span><span class="sxs-lookup"><span data-stu-id="65cab-112">Disable **Enable SSL** only if you are operating within a private cloud.</span></span>

        ![Tárfiók-hitelesítő adatok hozzáadása panel](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct3.png)

    5. <span data-ttu-id="65cab-114">Kattintson a **Hozzáadás** toostart hello feladat létrehozása hello tárolási fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="65cab-114">Click **Add** toostart hello job creation for hello storage account credential.</span></span> <span data-ttu-id="65cab-115">Értesítést fog kapni hello tárolási fiók hitelesítő adatait sikeres létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="65cab-115">You will be notified after hello storage account credential is successfully created.</span></span>

        ![Tárfiók-hitelesítő adatok – sikeres értesítés](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct5.png)

<span data-ttu-id="65cab-117">az újonnan létrehozott hello tárolási fiók hitelesítő adatait a hello listája alatt jelenik **Tárfiók hitelesítő adatainak**.</span><span class="sxs-lookup"><span data-stu-id="65cab-117">hello newly created storage account credential will be displayed under hello list of **Storage account credentials**.</span></span>

![Tárfiók hitelesítő adatainak listája](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct6.png)

