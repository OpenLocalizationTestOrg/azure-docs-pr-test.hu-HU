<!--author=alkohli last changed: 01/12/17-->

### <a name="tootake-a-backup"></a><span data-ttu-id="f1fea-101">a biztonsági mentés tootake</span><span class="sxs-lookup"><span data-stu-id="f1fea-101">tootake a backup</span></span>

1. <span data-ttu-id="f1fea-102">Nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f1fea-102">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="f1fea-103">Eszközök listája táblázatos hello, válassza ki, és kattintson a kívánt eszközre, majd kattintson **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="f1fea-103">From hello tabular listing of devices, select and click your device and then click **All settings**.</span></span> <span data-ttu-id="f1fea-104">A hello **beállítások** panelen, lépjen túl**beállítások > kezelése > biztonsági mentési házirend**.</span><span class="sxs-lookup"><span data-stu-id="f1fea-104">In hello **Settings** blade, go too**Settings > Manage > Backup policy**.</span></span>

    ![Biztonsági mentési házirend hozzáadása](./media/storsimple-8000-take-backup/step8takebu1.png)

2. <span data-ttu-id="f1fea-106">A hello **biztonsági mentési házirend** panelen kattintson a **+ házirend hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f1fea-106">In hello **Backup policy** blade, click **+ Add policy**.</span></span>

    ![Biztonsági mentési házirend hozzáadása](./media/storsimple-8000-take-backup/step8takebu2.png)

3. <span data-ttu-id="f1fea-108">A hello **hozzon létre a biztonsági mentési házirend** panelen adjon meg egy nevet, amely tartalmazza a biztonsági mentési házirend 3 és 150 karakter közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="f1fea-108">In hello **Create backup policy** blade, supply a name that contains between 3 and 150 characters for your backup policy.</span></span>

4. <span data-ttu-id="f1fea-109">Válassza ki a biztonsági mentés hello kötetek toobe.</span><span class="sxs-lookup"><span data-stu-id="f1fea-109">Select hello volumes toobe backed up.</span></span> <span data-ttu-id="f1fea-110">Egynél több kötetet választ ki, ha ezek a kötetek csoportosított együtt toocreate egy összeomlásbiztos biztonsági mentésbe.</span><span class="sxs-lookup"><span data-stu-id="f1fea-110">If you select more than one volume, these volumes are grouped together toocreate a crash-consistent backup.</span></span>

    ![Biztonsági mentési házirend hozzáadása](./media/storsimple-8000-take-backup/step8takebu4.png)

5. <span data-ttu-id="f1fea-112">Az **Első ütemezés hozzáadása** panelen:</span><span class="sxs-lookup"><span data-stu-id="f1fea-112">On **Add first schedule** blade:</span></span>

    1. <span data-ttu-id="f1fea-113">Válassza ki a biztonsági mentés hello típusát.</span><span class="sxs-lookup"><span data-stu-id="f1fea-113">Select hello type of backup.</span></span> <span data-ttu-id="f1fea-114">A gyorsabb visszaállítás érdekében válassza a **Helyi** pillanatfelvételt.</span><span class="sxs-lookup"><span data-stu-id="f1fea-114">For faster restores, select **Local** snapshot.</span></span> <span data-ttu-id="f1fea-115">Az adatrugalmasság érdekében válassza a **Felhőbeli** pillanatfelvételt.</span><span class="sxs-lookup"><span data-stu-id="f1fea-115">For data resiliency, select **Cloud** snapshot.</span></span>
    2. <span data-ttu-id="f1fea-116">Adja meg hello biztonsági mentés gyakoriságát percben, óra, nap vagy hét.</span><span class="sxs-lookup"><span data-stu-id="f1fea-116">Specify hello backup frequency in minutes, hours, days, or weeks.</span></span>
    3. <span data-ttu-id="f1fea-117">Adjon meg egy megőrzési időtartamot.</span><span class="sxs-lookup"><span data-stu-id="f1fea-117">Select a retention time.</span></span> <span data-ttu-id="f1fea-118">hello megőrzési lehetőségek hello biztonsági mentési gyakoriság függ.</span><span class="sxs-lookup"><span data-stu-id="f1fea-118">hello retention choices depend on hello backup frequency.</span></span> <span data-ttu-id="f1fea-119">Például egy napi házirend hello megőrzési is hetekben adható meg, míg havi házirend megőrzési értéke hónapon belül.</span><span class="sxs-lookup"><span data-stu-id="f1fea-119">For example, for a daily policy, hello retention can be specified in weeks, whereas retention for a monthly policy is in months.</span></span>
    4. <span data-ttu-id="f1fea-120">Válassza ki a kezdési időpontja és dátuma hello biztonsági mentési házirend hello.</span><span class="sxs-lookup"><span data-stu-id="f1fea-120">Select hello starting time and date for hello backup policy.</span></span>
    5. <span data-ttu-id="f1fea-121">Kattintson a **OK** toocreate hello biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="f1fea-121">Click **OK** toocreate hello backup policy.</span></span>

        ![Biztonsági mentési házirend hozzáadása](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. <span data-ttu-id="f1fea-123">Kattintson a **létrehozása** toostart hello biztonsági mentési házirend létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f1fea-123">Click **Create** toostart hello backup policy creation.</span></span> <span data-ttu-id="f1fea-124">Értesítést kap hello biztonsági mentési házirend sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="f1fea-124">You are notified when hello backup policy is successfully created.</span></span> <span data-ttu-id="f1fea-125">biztonsági mentési házirendek hello listája is frissül.</span><span class="sxs-lookup"><span data-stu-id="f1fea-125">hello list of backup policies is also updated.</span></span>
      
      ![Biztonsági mentési házirend hozzáadása](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      <span data-ttu-id="f1fea-127">Mostantól a biztonsági mentési szabályzat ütemezett biztonsági mentéseket hoz létre a kötetadatokról.</span><span class="sxs-lookup"><span data-stu-id="f1fea-127">You now have a backup policy that creates scheduled backups of your volume data.</span></span>




