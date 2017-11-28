<!--author=alkohli last changed: 02/10/17-->

#### <a name="to-add-a-storsimple-backup-policy"></a><span data-ttu-id="9d8b0-101">StorSimple biztonsági mentési házirend hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d8b0-101">To add a StorSimple backup policy</span></span>

1. <span data-ttu-id="9d8b0-102">A StorSimple-eszközben kattintson a **Biztonsági mentési szabályzat** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-102">Go to your StorSimple device and click **Backup policy**.</span></span>

2. <span data-ttu-id="9d8b0-103">A **Biztonsági mentési szabályzat** panelen kattintson a **+ Szabályzat hozzáadása** gombra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-103">In the **Backup policy** blade, click **+ Add policy** from the command bar.</span></span>
   
    ![Biztonsági mentési szabályzat hozzáadása](./media/storsimple-8000-add-backup-policy-u2/addbupol1.png)

3. <span data-ttu-id="9d8b0-105">Hajtsa végre a következő lépéseket a **Biztonsági mentési szabályzat létrehozása** panelen:</span><span class="sxs-lookup"><span data-stu-id="9d8b0-105">In the **Create backup policy** blade, do the following steps:</span></span>
   
   1. <span data-ttu-id="9d8b0-106">Az **Eszköz kiválasztása** mezőt a rendszer automatikusan kitölti a kiválasztott eszköz alapján.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-106">**Select device** is automatically populated based on the device you selected.</span></span>
   
   2. <span data-ttu-id="9d8b0-107">Adjon egy 3–150 karakter hosszúságú nevet a biztonsági mentési **szabályzat neveként**.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-107">Specify a backup **Policy name** that contains between 3 and 150 characters.</span></span> <span data-ttu-id="9d8b0-108">A szabályzat nem nevezhető át a létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-108">Once the policy is created, you cannot rename the policy.</span></span>
       
   3. <span data-ttu-id="9d8b0-109">Ha köteteket szeretne hozzárendelni ehhez a biztonsági mentési szabályzathoz, válassza a **Kötetek hozzáadása** lehetőséget, majd a kötetek táblázatos listájában a jelölőnégyzet(ek)re kattintva rendeljen hozzá egy vagy több kötetet a biztonsági mentési szabályzathoz.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-109">To assign volumes to this backup policy, select **Add volumes** and then from the tabular listing of volumes, click the check box(es) to assign one or more volumes to this backup policy.</span></span>

       ![Biztonsági mentési szabályzat hozzáadása](./media/storsimple-8000-add-backup-policy-u2/addbupol2.png)

   4. <span data-ttu-id="9d8b0-111">Ha ütemezést szeretne meghatározni a biztonsági mentési szabályzathoz, kattintson az **Első ütemezés** lehetőségre, majd módosítsa a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="9d8b0-111">To define a schedule for this backup policy, click **First schedule** and then modify the following parameters:</span></span>

       ![Biztonsági mentési szabályzat hozzáadása](./media/storsimple-8000-add-backup-policy-u2/addbupol3.png)

       1. <span data-ttu-id="9d8b0-113">A **Pillanatkép típusa** területen válassza a **Felhő** vagy a **Helyi** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-113">For **Snapshot type**, select **Cloud** or **Local**.</span></span>

       2. <span data-ttu-id="9d8b0-114">Határozza meg a biztonsági mentések gyakoriságát (adjon meg egy számot, majd válassza a **Nap** vagy **Hét** lehetőséget a legördülő listából).</span><span class="sxs-lookup"><span data-stu-id="9d8b0-114">Indicate the frequency of backups (specify a number and then choose **Days** or **Weeks** from the drop-down list.</span></span>

       3. <span data-ttu-id="9d8b0-115">Adja meg a megőrzés ütemezését.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-115">Enter a retention schedule.</span></span>

       4. <span data-ttu-id="9d8b0-116">Adja meg a biztonsági házirend indítási időpontját és dátumát.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-116">Enter a time and date for the backup policy to begin.</span></span>

       5. <span data-ttu-id="9d8b0-117">Kattintson az **OK** gombra az ütemezés meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-117">Click **OK** to define the schedule.</span></span>

   5. <span data-ttu-id="9d8b0-118">Biztonsági mentési szabályzat létrehozásához kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-118">Click **Create** to create a backup policy.</span></span>

       ![Biztonsági mentési szabályzat hozzáadása](./media/storsimple-8000-add-backup-policy-u2/addbupol4.png)
   
   6. <span data-ttu-id="9d8b0-120">A biztonsági mentési szabályzat létrehozásáról értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-120">You are notified when the backup policy is created.</span></span> <span data-ttu-id="9d8b0-121">Az újonnan felvett szabályzat megjelenik a **Biztonsági mentési szabályzat** panel táblázatos nézetében.</span><span class="sxs-lookup"><span data-stu-id="9d8b0-121">The newly added policy is displayed in the tabular view on the **Backup Policy** blade.</span></span>

       ![Biztonsági mentési szabályzat hozzáadása](./media/storsimple-8000-add-backup-policy-u2/addbupol7.png)

