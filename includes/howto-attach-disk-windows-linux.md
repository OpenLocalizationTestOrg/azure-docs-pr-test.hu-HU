


## <a name="attach-an-empty-disk"></a><span data-ttu-id="32f20-101">Üres lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="32f20-101">Attach an empty disk</span></span>
<span data-ttu-id="32f20-102">Üres lemez csatolása módja a egyszerű hozzá adatlemezt, mert az Azure létrehozza a .vhd fájlt, és azt a tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="32f20-102">Attaching an empty disk is a simple way to add a data disk, because Azure creates the .vhd file for you and stores it in the storage account.</span></span>

1. <span data-ttu-id="32f20-103">Kattintson a **virtuális gépek (klasszikus)**, majd válassza ki a megfelelő virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="32f20-103">Click **Virtual Machines (classic)**, and then select the appropriate VM.</span></span>

2. <span data-ttu-id="32f20-104">A beállítások menüben kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="32f20-104">In the Settings menu, click **Disks**.</span></span>

   ![Egy új üres lemez csatolása](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. <span data-ttu-id="32f20-106">A parancssávon kattintson **új Attach**.</span><span class="sxs-lookup"><span data-stu-id="32f20-106">On the command bar, click **Attach new**.</span></span>  
    <span data-ttu-id="32f20-107">A **új lemez csatolása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="32f20-107">The **Attach new disk** dialog box appears.</span></span>

    ![Új lemez csatolása](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    <span data-ttu-id="32f20-109">Írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="32f20-109">Fill in the following information:</span></span>
    - <span data-ttu-id="32f20-110">A **Fájlnév**, fogadja el az alapértelmezett nevet, vagy írja be a .vhd fájl egy másikat.</span><span class="sxs-lookup"><span data-stu-id="32f20-110">In **File Name**, accept the default name or type another one for the .vhd file.</span></span> <span data-ttu-id="32f20-111">Az adatok lemez egy automatikusan létrehozott nevet használ, akkor is, ha beírja a .vhd fájlt egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="32f20-111">The data disk uses an automatically generated name, even if you type another name for the .vhd file.</span></span>
    - <span data-ttu-id="32f20-112">Válassza ki a **típus** az adatok lemez.</span><span class="sxs-lookup"><span data-stu-id="32f20-112">Select the **Type** of the data disk.</span></span> <span data-ttu-id="32f20-113">Az összes virtuális gép támogatja a normál lemezeket.</span><span class="sxs-lookup"><span data-stu-id="32f20-113">All virtual machines support standard disks.</span></span> <span data-ttu-id="32f20-114">Több virtuális gép is támogatja a premium lemezek.</span><span class="sxs-lookup"><span data-stu-id="32f20-114">Many virtual machines also support premium disks.</span></span>
    - <span data-ttu-id="32f20-115">Válassza ki a **méret (GB)** az adatok lemez.</span><span class="sxs-lookup"><span data-stu-id="32f20-115">Select the **Size (GB)** of the data disk.</span></span>
    - <span data-ttu-id="32f20-116">A **állomás-gyorsítótárazása**, válasszon none, vagy csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="32f20-116">For **Host caching**, choose none or Read Only.</span></span>
    - <span data-ttu-id="32f20-117">Kattintson az OK gombra a befejezéshez.</span><span class="sxs-lookup"><span data-stu-id="32f20-117">Click OK to finish.</span></span>

4. <span data-ttu-id="32f20-118">Miután a adatlemez létrehozták, és a kapcsolódó, a virtuális lemezek szakaszában szerepel.</span><span class="sxs-lookup"><span data-stu-id="32f20-118">After the data disk is created and attached, it's listed in the disks section of the VM.</span></span>

   ![Sikeresen csatolta az új és üres adatlemez](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> <span data-ttu-id="32f20-120">Adatlemez hozzáadása után kell jelentkezzen be a virtuális Gépet, és végezze el a lemez inicializálását, hogy használható.</span><span class="sxs-lookup"><span data-stu-id="32f20-120">After you add a data disk, you need to log on to the VM and initialize the disk so that it can be used.</span></span>

## <a name="how-to-attach-an-existing-disk"></a><span data-ttu-id="32f20-121">Hogyan: meglévő lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="32f20-121">How to: Attach an existing disk</span></span>
<span data-ttu-id="32f20-122">Meglévő lemez csatlakoztatása esetén rendelkeznie kell egy tárfiókban elérhető .vhd-vel.</span><span class="sxs-lookup"><span data-stu-id="32f20-122">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span> <span data-ttu-id="32f20-123">Használja a [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) parancsmagot, hogy a .vhd fájl feltöltése a tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="32f20-123">Use the [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet to upload the .vhd file to the storage account.</span></span> <span data-ttu-id="32f20-124">Létrehozott, és a .vhd fájl feltöltése után hozzácsatolhat egy virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="32f20-124">After you've created and uploaded the .vhd file, you can attach it to a VM.</span></span>

1. <span data-ttu-id="32f20-125">Kattintson a **virtuális gépek (klasszikus)**, majd válassza ki a megfelelő virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="32f20-125">Click **Virtual Machines (classic)**, and then select the appropriate virtual machine.</span></span>

2. <span data-ttu-id="32f20-126">A beállítások menüben kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="32f20-126">In the Settings menu, click **Disks**.</span></span>

3. <span data-ttu-id="32f20-127">A parancssávon kattintson **Csatolás meglévő**.</span><span class="sxs-lookup"><span data-stu-id="32f20-127">On the command bar, click **Attach existing**.</span></span>

    ![Adatlemez csatolása](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. <span data-ttu-id="32f20-129">Kattintson a **hely**.</span><span class="sxs-lookup"><span data-stu-id="32f20-129">Click **Location**.</span></span> <span data-ttu-id="32f20-130">A rendelkezésre álló tár fiókokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="32f20-130">The available storage accounts display.</span></span> <span data-ttu-id="32f20-131">Ezután válassza ki a megfelelő tárolási fiók felsorolt.</span><span class="sxs-lookup"><span data-stu-id="32f20-131">Next, select an appropriate storage account from those listed.</span></span>

    ![Adja meg a storage-fiók](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. <span data-ttu-id="32f20-133">A **tárfiók** rendelkezik egy vagy több olyan tárolók, amelyek tartalmazzák a meghajtók (VHD-k).</span><span class="sxs-lookup"><span data-stu-id="32f20-133">A **Storage account** holds one or more containers that contain disk drives (vhds).</span></span> <span data-ttu-id="32f20-134">Válassza ki a megfelelő tárolót felsorolt.</span><span class="sxs-lookup"><span data-stu-id="32f20-134">Select the appropriate container from those listed.</span></span>

    ![Adja meg a virtuális gépek windows tároló](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. <span data-ttu-id="32f20-136">A **VHD-k** panel a tárolóban tárolt a merevlemez-meghajtók listája.</span><span class="sxs-lookup"><span data-stu-id="32f20-136">The **vhds** panel lists the disk drives held in the container.</span></span> <span data-ttu-id="32f20-137">Kattintson egy lemezt, majd válassza ki.</span><span class="sxs-lookup"><span data-stu-id="32f20-137">Click one of the disks, and then click Select.</span></span>

    ![Adja meg a lemezképet a virtuális gépek windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. <span data-ttu-id="32f20-139">A **meglévő lemez csatolása** panel megjeleníti újra, a helyen lévő tartalmazó a tárfiókot, a tároló és a kijelölt merevlemez (vhd) a virtuális géphez való hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="32f20-139">The **Attach existing disk** panel displays again, with the location containing the storage account, container, and selected hard disk (vhd) to add to the virtual machine.</span></span>

  <span data-ttu-id="32f20-140">Állítsa be **állomás-gyorsítótárazása** None vagy olvasási csak, majd kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="32f20-140">Set **Host caching** to none or Read only, then click OK.</span></span>

    ![Sikeresen csatolta adatlemez](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
