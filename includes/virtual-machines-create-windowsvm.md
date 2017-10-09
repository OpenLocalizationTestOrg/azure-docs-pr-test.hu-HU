1. <span data-ttu-id="930a0-101">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="930a0-101">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="930a0-102">Kattintson a bal felső hello kezdődően **új > számítási > Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="930a0-102">Starting in hello upper left, click **New > Compute > Windows Server 2016 Datacenter**.</span></span>

    ![Keresse meg az Azure Virtuálisgép-rendszerképekről toohello hello portálon](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. <span data-ttu-id="930a0-104">A Windows Server 2016 Datacenter hello hello klasszikus telepítési modell kiválasztása</span><span class="sxs-lookup"><span data-stu-id="930a0-104">On hello Windows Server 2016 Datacenter, select hello Classic deployment model.</span></span> <span data-ttu-id="930a0-105">Kattintson a Létrehozás gombra.</span><span class="sxs-lookup"><span data-stu-id="930a0-105">Click Create.</span></span>

    ![Képernyőkép a hello portálon elérhető hello Azure Virtuálisgép-rendszerképekről](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a><span data-ttu-id="930a0-107">1. Alapvető beállítások panel</span><span class="sxs-lookup"><span data-stu-id="930a0-107">1. Basics blade</span></span>

<span data-ttu-id="930a0-108">hello alapvető beállítások panel hello virtuális gép felügyeleti adatokat kér.</span><span class="sxs-lookup"><span data-stu-id="930a0-108">hello Basics blade requests administrative information for hello virtual machine.</span></span>

1. <span data-ttu-id="930a0-109">Adjon meg egy **neve** hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="930a0-109">Enter a **Name** for hello virtual machine.</span></span> <span data-ttu-id="930a0-110">Hello példában _HeroVM_ hello hello virtuális gép neve.</span><span class="sxs-lookup"><span data-stu-id="930a0-110">In hello example, _HeroVM_ is hello name of hello virtual machine.</span></span> <span data-ttu-id="930a0-111">hello nevének 1 – 15 karakter hosszúságúnak kell lennie, és nem tartalmazhat különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="930a0-111">hello name must be 1-15 characters long and it cannot contain special characters.</span></span>

2. <span data-ttu-id="930a0-112">Adjon meg egy **felhasználónév** és egy erős **jelszó** , amelyek használt toocreate helyi fiók a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="930a0-112">Enter a **User name** and a strong **Password** that are used toocreate a local account on hello VM.</span></span> <span data-ttu-id="930a0-113">hello helyi fiókot használja a tooand toosign hello VM kezelése.</span><span class="sxs-lookup"><span data-stu-id="930a0-113">hello local account is used toosign in tooand manage hello VM.</span></span> <span data-ttu-id="930a0-114">Hello példában _azureuser_ hello felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="930a0-114">In hello example, _azureuser_ is hello user name.</span></span>

 <span data-ttu-id="930a0-115">hello jelszót kell 8 – 123 karakter hosszú, és három hello négy következő bonyolultsági megfeleljen: egy kisbetű, egy nagybetű, egy számot és egy speciális karaktert.</span><span class="sxs-lookup"><span data-stu-id="930a0-115">hello password must be 8-123 characters long and meet three out of hello four following complexity requirements: one lower case character, one upper case character, one number, and one special character.</span></span> <span data-ttu-id="930a0-116">További információk a [felhasználónév- és jelszókövetelményekről](../articles/virtual-machines/windows/faq.md).</span><span class="sxs-lookup"><span data-stu-id="930a0-116">See more about [username and password requirements](../articles/virtual-machines/windows/faq.md).</span></span>

3. <span data-ttu-id="930a0-117">Hello **előfizetés** nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="930a0-117">hello **Subscription** is optional.</span></span> <span data-ttu-id="930a0-118">Gyakori beállítás a „használatalapú fizetés”.</span><span class="sxs-lookup"><span data-stu-id="930a0-118">One common setting is "Pay-As-You-Go".</span></span>

4. <span data-ttu-id="930a0-119">Válasszon ki egy létező **erőforráscsoport** vagy egy új hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="930a0-119">Select an existing **Resource group** or type hello name for a new one.</span></span> <span data-ttu-id="930a0-120">Hello példában _HeroVMRG_ hello hello erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="930a0-120">In hello example, _HeroVMRG_ is hello name of hello resource group.</span></span>

5. <span data-ttu-id="930a0-121">Válasszon egy Azure-adatközpontban **hely** hello VM toorun, ahová.</span><span class="sxs-lookup"><span data-stu-id="930a0-121">Select an Azure datacenter **Location** where you want hello VM toorun.</span></span> <span data-ttu-id="930a0-122">Hello példában **USA keleti régiója** hello helye.</span><span class="sxs-lookup"><span data-stu-id="930a0-122">In hello example, **East US** is hello location.</span></span>

6. <span data-ttu-id="930a0-123">Amikor elkészült, kattintson a **következő** toocontinue toohello következő panelen.</span><span class="sxs-lookup"><span data-stu-id="930a0-123">When you are done, click **Next** toocontinue toohello next blade.</span></span>

    ![Képernyőkép a hello-beállítások a hello alapvető beállítások panel az Azure virtuális gép konfigurálása](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a><span data-ttu-id="930a0-125">2. Méret panel</span><span class="sxs-lookup"><span data-stu-id="930a0-125">2. Size blade</span></span>

<span data-ttu-id="930a0-126">hello méret panelen azonosítja a virtuális gép hello hello konfigurációs részleteit, és felsorolja a különböző döntések, amely tartalmazza az operációs rendszer, processzorok, a lemez tárolási típus, és a becsült havi használati költségek száma.</span><span class="sxs-lookup"><span data-stu-id="930a0-126">hello Size blade identifies hello configuration details of hello VM, and lists various choices that include OS, number of processors, disk storage type, and estimated monthly usage costs.</span></span>  

<span data-ttu-id="930a0-127">Válassza ki a virtuális gép méretét, és kattintson a **válasszon** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="930a0-127">Choose a VM size, and then click **Select** toocontinue.</span></span> <span data-ttu-id="930a0-128">Ebben a példában _DS1_\__V2 szabványos_ van hello Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="930a0-128">In this example, _DS1_\__V2 Standard_ is hello VM size.</span></span>

  ![Képernyőfelvétel a hello méret panelen kiválasztható Azure Virtuálisgép-méretek jeleníti meg a hello](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a><span data-ttu-id="930a0-130">3. Beállítások panel</span><span class="sxs-lookup"><span data-stu-id="930a0-130">3. Settings blade</span></span>

<span data-ttu-id="930a0-131">hello-beállítások panelen a kérelmek tárolási és hálózati beállítások.</span><span class="sxs-lookup"><span data-stu-id="930a0-131">hello Settings blade requests storage and network options.</span></span> <span data-ttu-id="930a0-132">Elfogadhatja hello alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="930a0-132">You can accept hello default settings.</span></span> <span data-ttu-id="930a0-133">Az Azure szükség szerint létrehozza a megfelelő bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="930a0-133">Azure creates appropriate entries where necessary.</span></span>

<span data-ttu-id="930a0-134">Ha megfelelő méretet választott a virtuális gépnek, kipróbálhatja az Azure Premium Storage-ot – ehhez válassza a Prémium (SSD) elemet a Lemez típusa részen.</span><span class="sxs-lookup"><span data-stu-id="930a0-134">If you selected a virtual machine size that supports it, you can try Azure Premium Storage by selecting Premium (SSD) in Disk type.</span></span>

<span data-ttu-id="930a0-135">A módosítások végrehajtása után kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="930a0-135">When you're done making changes, click **OK**.</span></span>

## <a name="4-summary-blade"></a><span data-ttu-id="930a0-136">4. Összefoglalás panel</span><span class="sxs-lookup"><span data-stu-id="930a0-136">4. Summary blade</span></span>

<span data-ttu-id="930a0-137">hello összefoglaló panel hello előző paneleken megadott hello beállításokat sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="930a0-137">hello Summary blade lists hello settings specified in hello previous blades.</span></span> <span data-ttu-id="930a0-138">Kattintson a **OK** készen toomake hello kép közben.</span><span class="sxs-lookup"><span data-stu-id="930a0-138">Click **OK** when you're ready toomake hello image.</span></span>

 ![Hello virtuális gép megadott beállítások panel összefoglaló jelentést](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

<span data-ttu-id="930a0-140">Hello virtuális gép létrehozása után hello portál megjeleníti-e a hello új virtuális gépek **összes erőforrás**, és hello irányítópult hello virtuális gép egy csempe megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="930a0-140">After hello virtual machine is created, hello portal lists hello new virtual machine under **All resources**, and displays a tile of hello virtual machine on hello dashboard.</span></span> <span data-ttu-id="930a0-141">hello megfelelő felhőalapú szolgáltatás és a tárolási fiók is létrehozni és szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="930a0-141">hello corresponding cloud service and storage account also are created and listed.</span></span> <span data-ttu-id="930a0-142">Hello virtuális gép és a felhőalapú szolgáltatás automatikusan elindulnak, és azok állapottal jelenik meg, **futtató**.</span><span class="sxs-lookup"><span data-stu-id="930a0-142">Both hello virtual machine and cloud service are started automatically and their status is listed as **Running**.</span></span>

 ![Virtuálisgép-ügynök és hello végpontok hello virtuális gép konfigurálása](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
