1. <span data-ttu-id="07411-101">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="07411-101">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="07411-102">A bal felső sarokban kattintson az **Új > Számítás > Windows Server 2016 Datacenter** elemre.</span><span class="sxs-lookup"><span data-stu-id="07411-102">Starting in the upper left, click **New > Compute > Windows Server 2016 Datacenter**.</span></span>

    ![Navigálás az Azure virtuálisgép-rendszerképekre a portálon](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. <span data-ttu-id="07411-104">Válassza a klasszikus üzemi modellt a Windows Server 2016 Datacenter panelén.</span><span class="sxs-lookup"><span data-stu-id="07411-104">On the Windows Server 2016 Datacenter, select the Classic deployment model.</span></span> <span data-ttu-id="07411-105">Kattintson a Létrehozás gombra.</span><span class="sxs-lookup"><span data-stu-id="07411-105">Click Create.</span></span>

    ![Képernyőkép a portálon elérhető Azure virtuálisgép-rendszerképekről](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a><span data-ttu-id="07411-107">1. Alapvető beállítások panel</span><span class="sxs-lookup"><span data-stu-id="07411-107">1. Basics blade</span></span>

<span data-ttu-id="07411-108">Az Alapvető beállítások panelen felügyeleti információkat kell megadni a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="07411-108">The Basics blade requests administrative information for the virtual machine.</span></span>

1. <span data-ttu-id="07411-109">Adjon meg egy **nevet** a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="07411-109">Enter a **Name** for the virtual machine.</span></span> <span data-ttu-id="07411-110">A példában a virtuális gép neve _HeroVM_.</span><span class="sxs-lookup"><span data-stu-id="07411-110">In the example, _HeroVM_ is the name of the virtual machine.</span></span> <span data-ttu-id="07411-111">A névnek 1–15 karakter hosszúnak kell lennie, és nem tartalmazhat különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="07411-111">The name must be 1-15 characters long and it cannot contain special characters.</span></span>

2. <span data-ttu-id="07411-112">Adjon meg egy **felhasználónevet** és egy erős **jelszót**, amelyeket a helyi fióknak a virtuális gépen való létrehozásához használ a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="07411-112">Enter a **User name** and a strong **Password** that are used to create a local account on the VM.</span></span> <span data-ttu-id="07411-113">A helyi fiókkal jelentkezhet be a virtuális gépre és kezelheti azt.</span><span class="sxs-lookup"><span data-stu-id="07411-113">The local account is used to sign in to and manage the VM.</span></span> <span data-ttu-id="07411-114">A példában a felhasználónév az _azureuser_.</span><span class="sxs-lookup"><span data-stu-id="07411-114">In the example, _azureuser_ is the user name.</span></span>

 <span data-ttu-id="07411-115">A jelszónak 8–123 karakter hosszúnak kell lennie, és meg kell felelnie a következő négy összetettségi feltétel közül háromnak: egy kisbetű, egy nagybetű, egy szám és egy különleges karakter.</span><span class="sxs-lookup"><span data-stu-id="07411-115">The password must be 8-123 characters long and meet three out of the four following complexity requirements: one lower case character, one upper case character, one number, and one special character.</span></span> <span data-ttu-id="07411-116">További információk a [felhasználónév- és jelszókövetelményekről](../articles/virtual-machines/windows/faq.md).</span><span class="sxs-lookup"><span data-stu-id="07411-116">See more about [username and password requirements](../articles/virtual-machines/windows/faq.md).</span></span>

3. <span data-ttu-id="07411-117">Az **Előfizetés** nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="07411-117">The **Subscription** is optional.</span></span> <span data-ttu-id="07411-118">Gyakori beállítás a „használatalapú fizetés”.</span><span class="sxs-lookup"><span data-stu-id="07411-118">One common setting is "Pay-As-You-Go".</span></span>

4. <span data-ttu-id="07411-119">Válasszon ki egy létező **Erőforráscsoportot**, vagy adja meg egy új csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="07411-119">Select an existing **Resource group** or type the name for a new one.</span></span> <span data-ttu-id="07411-120">A példában az erőforráscsoport neve _HeroVMRG_.</span><span class="sxs-lookup"><span data-stu-id="07411-120">In the example, _HeroVMRG_ is the name of the resource group.</span></span>

5. <span data-ttu-id="07411-121">Válassza ki egy Azure-adatközpont **helyét**, ahol a virtuális gépet szeretné futtatni.</span><span class="sxs-lookup"><span data-stu-id="07411-121">Select an Azure datacenter **Location** where you want the VM to run.</span></span> <span data-ttu-id="07411-122">A példában a választott hely az **USA keleti régiója**.</span><span class="sxs-lookup"><span data-stu-id="07411-122">In the example, **East US** is the location.</span></span>

6. <span data-ttu-id="07411-123">Ha végzett, kattintson a **Tovább** gombra a következő panelre lépéshez.</span><span class="sxs-lookup"><span data-stu-id="07411-123">When you are done, click **Next** to continue to the next blade.</span></span>

    ![Képernyőkép az Alapvető beállítások panel beállításairól az Azure-beli virtuális gép konfigurálásához](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a><span data-ttu-id="07411-125">2. Méret panel</span><span class="sxs-lookup"><span data-stu-id="07411-125">2. Size blade</span></span>

<span data-ttu-id="07411-126">A Méret panel megadja a virtuális gép konfigurációs részleteit, és különböző választási lehetőségeket ajánl fel többek között az operációs rendszerre, a processzorok számára, a lemezes tárolás típusára és a becsült havi használati költségekre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="07411-126">The Size blade identifies the configuration details of the VM, and lists various choices that include OS, number of processors, disk storage type, and estimated monthly usage costs.</span></span>  

<span data-ttu-id="07411-127">Válassza ki a virtuális gép méretét, majd kattintson a **Kijelölés** elemre a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="07411-127">Choose a VM size, and then click **Select** to continue.</span></span> <span data-ttu-id="07411-128">Ebben a példában a virtuális gép mérete _DS1_\__V2 Standard_.</span><span class="sxs-lookup"><span data-stu-id="07411-128">In this example, _DS1_\__V2 Standard_ is the VM size.</span></span>

  ![Képernyőkép a Méret panelen kiválasztható Azure virtuálisgép-méretekről](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a><span data-ttu-id="07411-130">3. Beállítások panel</span><span class="sxs-lookup"><span data-stu-id="07411-130">3. Settings blade</span></span>

<span data-ttu-id="07411-131">A Beállítások panelen a tárolási és hálózati beállítások adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="07411-131">The Settings blade requests storage and network options.</span></span> <span data-ttu-id="07411-132">Elfogadhatja az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="07411-132">You can accept the default settings.</span></span> <span data-ttu-id="07411-133">Az Azure szükség szerint létrehozza a megfelelő bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="07411-133">Azure creates appropriate entries where necessary.</span></span>

<span data-ttu-id="07411-134">Ha megfelelő méretet választott a virtuális gépnek, kipróbálhatja az Azure Premium Storage-ot – ehhez válassza a Prémium (SSD) elemet a Lemez típusa részen.</span><span class="sxs-lookup"><span data-stu-id="07411-134">If you selected a virtual machine size that supports it, you can try Azure Premium Storage by selecting Premium (SSD) in Disk type.</span></span>

<span data-ttu-id="07411-135">A módosítások végrehajtása után kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="07411-135">When you're done making changes, click **OK**.</span></span>

## <a name="4-summary-blade"></a><span data-ttu-id="07411-136">4. Összefoglalás panel</span><span class="sxs-lookup"><span data-stu-id="07411-136">4. Summary blade</span></span>

<span data-ttu-id="07411-137">Az Összefoglalás panel felsorolja az előző paneleken megadott beállításokat.</span><span class="sxs-lookup"><span data-stu-id="07411-137">The Summary blade lists the settings specified in the previous blades.</span></span> <span data-ttu-id="07411-138">Kattintson az **OK** gombra, amikor készen áll a rendszerkép létrehozására.</span><span class="sxs-lookup"><span data-stu-id="07411-138">Click **OK** when you're ready to make the image.</span></span>

 ![Az Összefoglalás panel jelentése, amely megadja a virtuális gép beállításait](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

<span data-ttu-id="07411-140">A virtuális gép létrejötte után a portál megjeleníti az új virtuális gépet a **Minden erőforrás** részben, és megjeleníti a virtuális gép csempéjét az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="07411-140">After the virtual machine is created, the portal lists the new virtual machine under **All resources**, and displays a tile of the virtual machine on the dashboard.</span></span> <span data-ttu-id="07411-141">A rendszer emellett létrehozza és felsorolja a megfelelő felhőszolgáltatást és tárfiókot is.</span><span class="sxs-lookup"><span data-stu-id="07411-141">The corresponding cloud service and storage account also are created and listed.</span></span> <span data-ttu-id="07411-142">Mind a virtuális gép, mind a felhőszolgáltatás automatikusan elindul, és **Fut** állapotúként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="07411-142">Both the virtual machine and cloud service are started automatically and their status is listed as **Running**.</span></span>

 ![A virtuálisgép-ügynök és a virtuális gép végpontjainak konfigurálása](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
