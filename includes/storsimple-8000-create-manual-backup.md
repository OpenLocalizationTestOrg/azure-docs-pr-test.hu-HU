
<!--author=alkohli last changed: 01/20/2017-->

#### <a name="to-create-a-manual-backup"></a><span data-ttu-id="acb38-101">Manuális biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="acb38-101">To create a manual backup</span></span>

1. <span data-ttu-id="acb38-102">A StorSimple-eszközkezelő szolgáltatásban kattintson az **Eszközök** elemre.</span><span class="sxs-lookup"><span data-stu-id="acb38-102">Go to your StorSimple Device Manager service and then click **Devices**.</span></span> <span data-ttu-id="acb38-103">Az eszközök táblázatos listájából válassza ki az eszközt.</span><span class="sxs-lookup"><span data-stu-id="acb38-103">From the tabular listing of devices, select your device.</span></span> <span data-ttu-id="acb38-104">Lépjen a **Beállítások > Kezelés > Biztonsági mentési házirendek** részhez.</span><span class="sxs-lookup"><span data-stu-id="acb38-104">Go to **Settings > Manage > Backup policies**.</span></span>

2. <span data-ttu-id="acb38-105">A **Biztonsági mentési házirendek** panelen táblázatos formában látható az összes biztonsági mentési házirend, beleértve az arra a kötetre vonatkozó házirendet, amelyről a biztonsági mentést el szeretné készíteni.</span><span class="sxs-lookup"><span data-stu-id="acb38-105">The **Backup policies** blade lists all the backup policies in a tabular format, including the policy for the volume that you want to back up.</span></span> <span data-ttu-id="acb38-106">Válassza ki az azon kötethez tartozó házirendet, amelyről biztonsági másolatot szeretne készíteni, majd kattintson a jobb gombbal a helyi menü megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="acb38-106">Select the policy associated with the volume you want to back up and right-click to invoke the context menu.</span></span> <span data-ttu-id="acb38-107">A legördülő listában kattintson a **Biztonsági másolat készítése** parancsra.</span><span class="sxs-lookup"><span data-stu-id="acb38-107">From the dropdown list, select **Back up now**.</span></span>

    ![Manuális biztonsági mentés létrehozása](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. <span data-ttu-id="acb38-109">Hajtsa végre a következő lépéseket a **Biztonsági másolat készítése** panelen:</span><span class="sxs-lookup"><span data-stu-id="acb38-109">In the **Back up now** blade, do the following steps:</span></span>

    1. <span data-ttu-id="acb38-110">A legördülő menüben válassza ki a megfelelő **pillanatképtípust**: **helyi** vagy **felhőbeli**.</span><span class="sxs-lookup"><span data-stu-id="acb38-110">Choose the appropriate **Snapshot type** from the dropdown list: **Local** snapshot or **Cloud** snapshot.</span></span> <span data-ttu-id="acb38-111">Gyors biztonsági mentéshez és visszaállításhoz válassza a helyi pillanatképet, az adatrugalmassághoz pedig a felhőbeli pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="acb38-111">Select local snapshot for fast backups or restores, and cloud snapshot for data resiliency.</span></span>

        ![Manuális biztonsági mentés létrehozása](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. <span data-ttu-id="acb38-113">Kattintson az **OK** gombra a pillanatképet létrehozó feladat elindításához.</span><span class="sxs-lookup"><span data-stu-id="acb38-113">Click **OK** to start a job to create a snapshot.</span></span> <span data-ttu-id="acb38-114">A feladat sikeres létrejöttét a lap tetején megjelenő értesítés jelzi.</span><span class="sxs-lookup"><span data-stu-id="acb38-114">You will see a notification at the top of the page after the job is successfully created.</span></span>

        ![Manuális biztonsági mentés létrehozása](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. <span data-ttu-id="acb38-116">A feladat figyeléséhez kattintson az értesítésre.</span><span class="sxs-lookup"><span data-stu-id="acb38-116">To monitor the job, click the notification.</span></span> <span data-ttu-id="acb38-117">Ezzel továbblép a **Feladatok** panelre, ahol megtekintheti a feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="acb38-117">This takes you to the **Jobs** blade where you can view the job progress.</span></span>


5. <span data-ttu-id="acb38-118">Ha a biztonsági mentési feladat befejeződött, nyissa meg a **Biztonságimásolat-katalógus** lapot.</span><span class="sxs-lookup"><span data-stu-id="acb38-118">After the backup job is finished, go to the **Backup catalog** tab.</span></span>

6. <span data-ttu-id="acb38-119">Állítsa be a szűrőket úgy, hogy a megfelelő eszköz, biztonsági mentési házirend és időtartomány jelenjen meg.</span><span class="sxs-lookup"><span data-stu-id="acb38-119">Set the filter selections to the appropriate device, backup policy, and time range.</span></span> <span data-ttu-id="acb38-120">Ezután a biztonsági mentés megjelenik a katalógusban szereplő biztonságimentés-készletek listájában.</span><span class="sxs-lookup"><span data-stu-id="acb38-120">The backup should appear in the list of backup sets that is displayed in the catalog.</span></span>

