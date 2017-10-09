
<!--author=alkohli last changed: 01/20/2017-->

#### <a name="toocreate-a-manual-backup"></a><span data-ttu-id="5b148-101">Manuális biztonsági mentés toocreate</span><span class="sxs-lookup"><span data-stu-id="5b148-101">toocreate a manual backup</span></span>

1. <span data-ttu-id="5b148-102">Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="5b148-102">Go tooyour StorSimple Device Manager service and then click **Devices**.</span></span> <span data-ttu-id="5b148-103">Hello táblázatos eszközök listája jelölje ki azt az eszközt.</span><span class="sxs-lookup"><span data-stu-id="5b148-103">From hello tabular listing of devices, select your device.</span></span> <span data-ttu-id="5b148-104">Nyissa meg túl**beállítások > kezelés > biztonsági mentési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="5b148-104">Go too**Settings > Manage > Backup policies**.</span></span>

2. <span data-ttu-id="5b148-105">Hello **biztonsági mentési házirendek** panel összes hello biztonsági mentési házirendek táblázatos formában, többek között a hello házirend hello kötet kívánt tooback mentése sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="5b148-105">hello **Backup policies** blade lists all hello backup policies in a tabular format, including hello policy for hello volume that you want tooback up.</span></span> <span data-ttu-id="5b148-106">Válassza ki a tooback akarja hello kötet, és kattintson a jobb gombbal tooinvoke hello helyi menü társított hello házirendet.</span><span class="sxs-lookup"><span data-stu-id="5b148-106">Select hello policy associated with hello volume you want tooback up and right-click tooinvoke hello context menu.</span></span> <span data-ttu-id="5b148-107">Hello legördülő listából válassza ki a **biztonsági másolat készítése**.</span><span class="sxs-lookup"><span data-stu-id="5b148-107">From hello dropdown list, select **Back up now**.</span></span>

    ![Manuális biztonsági mentés létrehozása](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. <span data-ttu-id="5b148-109">A hello **biztonsági másolat készítése** panelen hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5b148-109">In hello **Back up now** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="5b148-110">Válassza ki a megfelelő hello **pillanatkép-típus** hello legördülő listából: **helyi** pillanatkép vagy **felhő** pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="5b148-110">Choose hello appropriate **Snapshot type** from hello dropdown list: **Local** snapshot or **Cloud** snapshot.</span></span> <span data-ttu-id="5b148-111">Gyors biztonsági mentéshez és visszaállításhoz válassza a helyi pillanatképet, az adatrugalmassághoz pedig a felhőbeli pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="5b148-111">Select local snapshot for fast backups or restores, and cloud snapshot for data resiliency.</span></span>

        ![Manuális biztonsági mentés létrehozása](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. <span data-ttu-id="5b148-113">Kattintson a **OK** toostart egy feladat toocreate pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="5b148-113">Click **OK** toostart a job toocreate a snapshot.</span></span> <span data-ttu-id="5b148-114">Hello oldal hello tetején értesítést hello feladat sikeres létrehozása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5b148-114">You will see a notification at hello top of hello page after hello job is successfully created.</span></span>

        ![Manuális biztonsági mentés létrehozása](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. <span data-ttu-id="5b148-116">toomonitor hello feladat, kattintson a hello értesítés.</span><span class="sxs-lookup"><span data-stu-id="5b148-116">toomonitor hello job, click hello notification.</span></span> <span data-ttu-id="5b148-117">Ezzel megnyitná toohello **feladatok** panel, ahol megtekintheti a hello feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="5b148-117">This takes you toohello **Jobs** blade where you can view hello job progress.</span></span>


5. <span data-ttu-id="5b148-118">Miután hello biztonsági mentési feladat befejeződött, nyissa meg toohello **biztonságimásolat-katalógus** fülre.</span><span class="sxs-lookup"><span data-stu-id="5b148-118">After hello backup job is finished, go toohello **Backup catalog** tab.</span></span>

6. <span data-ttu-id="5b148-119">Hello szűrő beállításokat toohello megfelelő eszköz, a biztonsági mentési házirend és időtartomány beállítása.</span><span class="sxs-lookup"><span data-stu-id="5b148-119">Set hello filter selections toohello appropriate device, backup policy, and time range.</span></span> <span data-ttu-id="5b148-120">hello biztonsági mentés meg kell jelennie hello listájában, hello katalógust a biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="5b148-120">hello backup should appear in hello list of backup sets that is displayed in hello catalog.</span></span>

