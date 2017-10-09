


## <a name="attach-an-empty-disk"></a><span data-ttu-id="c9ea3-101">Üres lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="c9ea3-101">Attach an empty disk</span></span>
<span data-ttu-id="c9ea3-102">Üres lemez csatolása egy egyszerű módon tooadd az adatok lemezre, mivel Azure hello .vhd fájl létrehozza, és azt hello tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-102">Attaching an empty disk is a simple way tooadd a data disk, because Azure creates hello .vhd file for you and stores it in hello storage account.</span></span>

1. <span data-ttu-id="c9ea3-103">Kattintson a **virtuális gépek (klasszikus)**, és ezután válasszon hello megfelelő méretű.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-103">Click **Virtual Machines (classic)**, and then select hello appropriate VM.</span></span>

2. <span data-ttu-id="c9ea3-104">Hello-beállítások menüjében kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-104">In hello Settings menu, click **Disks**.</span></span>

   ![Egy új üres lemez csatolása](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. <span data-ttu-id="c9ea3-106">A hello parancssávon kattintson **új Attach**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-106">On hello command bar, click **Attach new**.</span></span>  
    <span data-ttu-id="c9ea3-107">Hello **új lemez csatolása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-107">hello **Attach new disk** dialog box appears.</span></span>

    ![Új lemez csatolása](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    <span data-ttu-id="c9ea3-109">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-109">Fill in hello following information:</span></span>
    - <span data-ttu-id="c9ea3-110">A **Fájlnév**fogadja el a hello alapértelmezett nevet, vagy írja be a hello .vhd fájl egy másikat.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-110">In **File Name**, accept hello default name or type another one for hello .vhd file.</span></span> <span data-ttu-id="c9ea3-111">hello adatlemez egy automatikusan létrehozott nevet használja, akkor is, ha beírja hello .vhd fájl egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-111">hello data disk uses an automatically generated name, even if you type another name for hello .vhd file.</span></span>
    - <span data-ttu-id="c9ea3-112">Jelölje be hello **típus** hello adatok lemez.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-112">Select hello **Type** of hello data disk.</span></span> <span data-ttu-id="c9ea3-113">Az összes virtuális gép támogatja a normál lemezeket.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-113">All virtual machines support standard disks.</span></span> <span data-ttu-id="c9ea3-114">Több virtuális gép is támogatja a premium lemezek.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-114">Many virtual machines also support premium disks.</span></span>
    - <span data-ttu-id="c9ea3-115">Jelölje be hello **méret (GB)** hello adatok lemez.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-115">Select hello **Size (GB)** of hello data disk.</span></span>
    - <span data-ttu-id="c9ea3-116">A **állomás-gyorsítótárazása**, válasszon none, vagy csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-116">For **Host caching**, choose none or Read Only.</span></span>
    - <span data-ttu-id="c9ea3-117">Kattintson az OK toofinish.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-117">Click OK toofinish.</span></span>

4. <span data-ttu-id="c9ea3-118">Miután hello adatlemez létrehozni és csatolni, hello VM hello lemezek szakaszában szerepel.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-118">After hello data disk is created and attached, it's listed in hello disks section of hello VM.</span></span>

   ![Sikeresen csatolta az új és üres adatlemez](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> <span data-ttu-id="c9ea3-120">Adatlemez hozzáadása után a virtuális gép toohello toolog szükségesek, és hello lemez inicializálása, hogy használatba vehető.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-120">After you add a data disk, you need toolog on toohello VM and initialize hello disk so that it can be used.</span></span>

## <a name="how-to-attach-an-existing-disk"></a><span data-ttu-id="c9ea3-121">Hogyan: meglévő lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="c9ea3-121">How to: Attach an existing disk</span></span>
<span data-ttu-id="c9ea3-122">Meglévő lemez csatlakoztatása esetén rendelkeznie kell egy tárfiókban elérhető .vhd-vel.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-122">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span> <span data-ttu-id="c9ea3-123">Használjon hello [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) parancsmag tooupload hello .vhd fájl toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-123">Use hello [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet tooupload hello .vhd file toohello storage account.</span></span> <span data-ttu-id="c9ea3-124">Miután létrehozott és hello .vhd fájl feltöltése, csatolhatók tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-124">After you've created and uploaded hello .vhd file, you can attach it tooa VM.</span></span>

1. <span data-ttu-id="c9ea3-125">Kattintson a **virtuális gépek (klasszikus)**, és ezután hello az válassza ki a megfelelő virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-125">Click **Virtual Machines (classic)**, and then select hello appropriate virtual machine.</span></span>

2. <span data-ttu-id="c9ea3-126">Hello-beállítások menüjében kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-126">In hello Settings menu, click **Disks**.</span></span>

3. <span data-ttu-id="c9ea3-127">A hello parancssávon kattintson **Csatolás meglévő**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-127">On hello command bar, click **Attach existing**.</span></span>

    ![Adatlemez csatolása](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. <span data-ttu-id="c9ea3-129">Kattintson a **hely**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-129">Click **Location**.</span></span> <span data-ttu-id="c9ea3-130">hello rendelkezésre álló tár fiókokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-130">hello available storage accounts display.</span></span> <span data-ttu-id="c9ea3-131">Ezután válassza ki a megfelelő tárolási fiók felsorolt.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-131">Next, select an appropriate storage account from those listed.</span></span>

    ![Adja meg a storage-fiók](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. <span data-ttu-id="c9ea3-133">A **tárfiók** rendelkezik egy vagy több olyan tárolók, amelyek tartalmazzák a meghajtók (VHD-k).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-133">A **Storage account** holds one or more containers that contain disk drives (vhds).</span></span> <span data-ttu-id="c9ea3-134">Válassza ki a megfelelő tárolót hello felsorolt.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-134">Select hello appropriate container from those listed.</span></span>

    ![Adja meg a virtuális gépek windows tároló](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. <span data-ttu-id="c9ea3-136">Hello **VHD-k** panel hello tárolóban tárolt hello lemezmeghajtók sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-136">hello **vhds** panel lists hello disk drives held in hello container.</span></span> <span data-ttu-id="c9ea3-137">Kattintson hello lemezek közül, majd válassza ki.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-137">Click one of hello disks, and then click Select.</span></span>

    ![Adja meg a lemezképet a virtuális gépek windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. <span data-ttu-id="c9ea3-139">Hello **meglévő lemez csatolása** panel újra, hello helyen lévő hello tárfiókot, a tároló és a kijelölt merevlemez (vhd) tooadd toohello virtuális gépet tartalmazó megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-139">hello **Attach existing disk** panel displays again, with hello location containing hello storage account, container, and selected hard disk (vhd) tooadd toohello virtual machine.</span></span>

  <span data-ttu-id="c9ea3-140">Állítsa be **állomás-gyorsítótárazása** toonone vagy olvasási csak, majd kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-140">Set **Host caching** toonone or Read only, then click OK.</span></span>

    ![Sikeresen csatolta adatlemez](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
