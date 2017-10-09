


<span data-ttu-id="7473e-101">Rendelkezésre állási készlet nyújt segítséget, mint tartani a virtuális gépek állásidő, során rendelkezésre álló karbantartás során.</span><span class="sxs-lookup"><span data-stu-id="7473e-101">An availability set helps keep your virtual machines available during downtime, such as during maintenance.</span></span> <span data-ttu-id="7473e-102">Hello szükséges redundancia toomaintain rendelkezésre állási hello az alkalmazások és szolgáltatások a virtuális gépet futtató két vagy több hasonló módon konfigurált virtuális gépek elhelyezése egy rendelkezésre állási csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7473e-102">Placing two or more similarly configured virtual machines in an availability set creates hello redundancy needed toomaintain availability of hello applications or services that your virtual machine runs.</span></span> <span data-ttu-id="7473e-103">Ez működésével kapcsolatos részletekért lásd: [hello virtuális gépek rendelkezésre állásának kezelése][Manage hello availability of virtual machines].</span><span class="sxs-lookup"><span data-stu-id="7473e-103">For details about how this works, see [Manage hello availability of virtual machines][Manage hello availability of virtual machines].</span></span>

<span data-ttu-id="7473e-104">A bevált gyakorlat toouse rendelkezésre állási készletek és a terheléselosztás végpontok toohelp gondoskodjon arról, hogy az alkalmazás mindig elérhető, és fut. hatékony.</span><span class="sxs-lookup"><span data-stu-id="7473e-104">It's a best practice toouse both availability sets and load-balancing endpoints toohelp ensure that your application is always available and running efficiently.</span></span> <span data-ttu-id="7473e-105">További információk az elosztott terhelésű végpont: [Azure infrastruktúra-szolgáltatásokat a terheléselosztás][Load balancing for Azure infrastructure services].</span><span class="sxs-lookup"><span data-stu-id="7473e-105">For details about load-balanced endpoints, see [Load balancing for Azure infrastructure services][Load balancing for Azure infrastructure services].</span></span>

<span data-ttu-id="7473e-106">Klasszikus virtuális gépeket adhat hozzá a rendelkezésre állási készlet két lehetőség egyikét használva:</span><span class="sxs-lookup"><span data-stu-id="7473e-106">You can add classic virtual machines into an availability set by using one of two options:</span></span>

* <span data-ttu-id="7473e-107">[1. lehetőség: Hozzon létre egy virtuális gépet, és a rendelkezésre állási készlet: hello azonos idő][Option 1: Create a virtual machine and an availability set at hello same time].</span><span class="sxs-lookup"><span data-stu-id="7473e-107">[Option 1: Create a virtual machine and an availability set at hello same time][Option 1: Create a virtual machine and an availability set at hello same time].</span></span> <span data-ttu-id="7473e-108">Adja hozzá az új virtuális gépek toohello azon virtuális gépek létrehozásakor állítsa be.</span><span class="sxs-lookup"><span data-stu-id="7473e-108">Then, add new virtual machines toohello set when you create those virtual machines.</span></span>
* <span data-ttu-id="7473e-109">[2. lehetőség: Hozzáadása egy meglévő virtuális gép tooan rendelkezésre állási csoportot][Option 2: Add an existing virtual machine tooan availability set].</span><span class="sxs-lookup"><span data-stu-id="7473e-109">[Option 2: Add an existing virtual machine tooan availability set][Option 2: Add an existing virtual machine tooan availability set].</span></span>

> [!NOTE]
> <span data-ttu-id="7473e-110">Hello a klasszikus modellben tooput kívánt virtuális gépek hello azonos rendelkezésre állási csoportba kell tartoznia toohello ugyanaz a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7473e-110">In hello classic model, virtual machines that you want tooput in hello same availability set must belong toohello same cloud service.</span></span>
> 
> 

## <span data-ttu-id="7473e-111"><a id="createset"></a>1. lehetőség: hozzon létre egy virtuális gépet, és a rendelkezésre állási készlet: hello azonos idő</span><span class="sxs-lookup"><span data-stu-id="7473e-111"><a id="createset"> </a>Option 1: Create a virtual machine and an availability set at hello same time</span></span>
<span data-ttu-id="7473e-112">Vagy hello Azure-portálon is használhatja, vagy az Azure PowerShell-parancsok toodo ez.</span><span class="sxs-lookup"><span data-stu-id="7473e-112">You can use either hello Azure portal or Azure PowerShell commands toodo this.</span></span>

<span data-ttu-id="7473e-113">toouse hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="7473e-113">toouse hello Azure portal:</span></span>

1. <span data-ttu-id="7473e-114">Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7473e-114">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7473e-115">Hello központ menüben kattintson a **+ új**, és kattintson a **virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="7473e-115">On hello hub menu, click **+ New**, and then click **Virtual Machine**.</span></span>
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. <span data-ttu-id="7473e-117">Válassza ki a hello piactér virtuálisgép-lemezkép toouse kívánja.</span><span class="sxs-lookup"><span data-stu-id="7473e-117">Select hello Marketplace virtual machine image you wish toouse.</span></span> <span data-ttu-id="7473e-118">Választhat toocreate egy Linux vagy a Windows virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="7473e-118">You can choose toocreate a Linux or Windows virtual machine.</span></span>
4. <span data-ttu-id="7473e-119">A hello kiválasztott virtuális gépet, győződjön meg arról, hogy hello telepítési modell értéke túl**klasszikus** majd **létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7473e-119">For hello selected virtual machine, verify that hello deployment model is set too**Classic** and then click **Create**</span></span>
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. <span data-ttu-id="7473e-121">Adja meg a virtuális gép nevét, felhasználónevet és jelszót (a Windows gépek) vagy (a Linux-gépek) nyilvános SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7473e-121">Enter a virtual machine name, user name and password (for Windows machines) or SSH public key (for Linux machines).</span></span> 
6. <span data-ttu-id="7473e-122">Válassza ki a hello Virtuálisgép-méretet, és kattintson a **válasszon** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="7473e-122">Choose hello VM size and then click **Select** toocontinue.</span></span>
7. <span data-ttu-id="7473e-123">Válasszon **opcionális konfigurációs > rendelkezésre állási csoport**, és válassza ki a hello rendelkezésre állási csoport tooadd hello a virtuális gépet kívánja.</span><span class="sxs-lookup"><span data-stu-id="7473e-123">Choose **Optional Configuration > Availability set**, and select hello availability set you wish tooadd hello virtual machine to.</span></span>
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. <span data-ttu-id="7473e-125">Tekintse át a konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7473e-125">Review your configuration settings.</span></span> <span data-ttu-id="7473e-126">Amikor elkészült, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7473e-126">When you're done, click **Create**.</span></span>
9. <span data-ttu-id="7473e-127">Amíg az Azure létrehozza a virtuális gép, előrehaladásának hello alatt **virtuális gépek** hello központ menüben.</span><span class="sxs-lookup"><span data-stu-id="7473e-127">While Azure creates your virtual machine, you can track hello progress under **Virtual Machines** in hello hub menu.</span></span>

<span data-ttu-id="7473e-128">toouse Azure PowerShell-parancsok toocreate Azure virtuális gép és tooa új vagy meglévő rendelkezésre állási csoportot adja hozzá, lásd: [használata Azure PowerShell toocreate és előre konfigurálása a Windows-alapú virtuális gépek](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="7473e-128">toouse Azure PowerShell commands toocreate an Azure virtual machine and add it tooa new or existing availability set, see [Use Azure PowerShell toocreate and preconfigure Windows-based virtual machines](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="7473e-129"><a id="addmachine"></a>2. lehetőség: egy meglévő virtuális gép tooan rendelkezésre állási csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7473e-129"><a id="addmachine"> </a>Option 2: Add an existing virtual machine tooan availability set</span></span>
<span data-ttu-id="7473e-130">Hello Azure-portálon, a meglévő klasszikus virtuális gépek tooan meglévő rendelkezésre állási csoport hozzáadása, vagy hozzon létre egy újat a számukra.</span><span class="sxs-lookup"><span data-stu-id="7473e-130">In hello Azure portal, you can add existing classic virtual machines tooan existing availability set or create a new one for them.</span></span> <span data-ttu-id="7473e-131">(Ne feledje, hogy a virtuális gépek hello azonos rendelkezésre állási csoport hello kell tartozniuk toohello ugyanaz a felhőalapú szolgáltatás.) hello lépéseket majdnem azonos hello szövegrészt.</span><span class="sxs-lookup"><span data-stu-id="7473e-131">(Keep in mind that hello virtual machines in hello same availability set must belong toohello same cloud service.) hello steps are almost hello same.</span></span> <span data-ttu-id="7473e-132">Az Azure PowerShell hello virtuális gép tooan meglévő rendelkezésre állási csoport is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="7473e-132">With Azure PowerShell, you can add hello virtual machine tooan existing availability set.</span></span>

1. <span data-ttu-id="7473e-133">Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7473e-133">If you have not already done so, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7473e-134">Hello központ menüben kattintson a **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="7473e-134">On hello Hub menu, click **Virtual Machines (classic)**.</span></span>
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. <span data-ttu-id="7473e-136">A virtuális gépek hello listáról válassza ki a hello virtuális gép, amelyet az tooadd toohello set hello nevét.</span><span class="sxs-lookup"><span data-stu-id="7473e-136">From hello list of virtual machines, select hello name of hello virtual machine that you want tooadd toohello set.</span></span>
4. <span data-ttu-id="7473e-137">Válasszon **rendelkezésre állási csoport** hello virtuális gépről **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7473e-137">Choose **Availability set** from hello virtual machine **Settings**.</span></span>
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. <span data-ttu-id="7473e-139">Válassza ki a hello rendelkezésre állási csoport tooadd hello a virtuális gépet kívánja.</span><span class="sxs-lookup"><span data-stu-id="7473e-139">Select hello availability set you wish tooadd hello virtual machine to.</span></span> <span data-ttu-id="7473e-140">hello virtuális gép kell tartozniuk toohello ugyanaz a felhőalapú szolgáltatás, hello rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="7473e-140">hello virtual machine must belong toohello same cloud service as hello availability set.</span></span>
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. <span data-ttu-id="7473e-142">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="7473e-142">Click **Save**.</span></span>

<span data-ttu-id="7473e-143">toouse Azure PowerShell-parancsok, nyisson meg egy rendszergazdai Azure PowerShell-munkamenetet, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="7473e-143">toouse Azure PowerShell commands, open an administrator-level Azure PowerShell session and run hello following command.</span></span> <span data-ttu-id="7473e-144">A helyőrzők hello (például &lt;VmCloudServiceName&gt;), cserélje le a mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, hello megfelelő neveket.</span><span class="sxs-lookup"><span data-stu-id="7473e-144">For hello placeholders (such as &lt;VmCloudServiceName&gt;), replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> <span data-ttu-id="7473e-145">hello virtuális gép újraindítása toobe toofinish toohello rendelkezésre állási csoport hozzáadásával rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="7473e-145">hello virtual machine might have toobe restarted toofinish adding it toohello availability set.</span></span>
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at hello same time]: #createset
[Option 2: Add an existing virtual machine tooan availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage hello availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

