


## <a name="using-vm-extensions"></a><span data-ttu-id="314c8-101">Virtuálisgép-bővítmények használatával</span><span class="sxs-lookup"><span data-stu-id="314c8-101">Using VM Extensions</span></span>
<span data-ttu-id="314c8-102">Az Azure Virtuálisgép-bővítmények megvalósítása viselkedések vagy szolgáltatásokat vagy más programok Azure virtuális gépeken használata (például hello **WebDeployForVSDevTest** bővítmény lehetővé teszi, hogy a Visual Studio tooWeb telepítés megoldásokat az Azure virtuális gépen), vagy adjon meg hello lehetőségét, hogy a virtuális gép toosupport hello toointeract néhány más viselkedés (például használhatja hello VM eléréséhez kapcsolódó kiegészítő mezőket PowerShell hello Azure CLI és REST ügyfelek tooreset, de távelérési értékek módosítása az Azure virtuális gépen).</span><span class="sxs-lookup"><span data-stu-id="314c8-102">Azure VM Extensions implement behaviors or features that either help other programs work on Azure VMs (for example, hello **WebDeployForVSDevTest** extension allows Visual Studio tooWeb Deploy solutions on your Azure VM) or provide hello ability for you toointeract with hello VM toosupport some other behavior (for example, you can use hello VM Access extensions from PowerShell, hello Azure CLI, and REST clients tooreset or modify remote access values on your Azure VM).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="314c8-103">Bővítmények hello funkcióihoz támogatják-e teljes listáját lásd: [Azure Virtuálisgép-bővítmények és a szolgáltatások](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="314c8-103">For a complete list of extensions by hello features they support, see [Azure VM Extensions and Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="314c8-104">Mivel minden egyes Virtuálisgép-bővítmény támogatja az egy adott funkcióhoz, pontosan mit, illetve használhat nem kiterjesztési függ hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="314c8-104">Because each VM extension supports a specific feature, exactly what you can and cannot do with an extension depends on hello extension.</span></span> <span data-ttu-id="314c8-105">-Módosítása előtt a virtuális gép mindenképpen elolvasta hello toouse kívánt Virtuálisgép-bővítmény hello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="314c8-105">Therefore, before modifying your VM, make sure you have read hello documentation for hello VM Extension you want toouse.</span></span> <span data-ttu-id="314c8-106">Egyes Virtuálisgép-bővítmények nem támogatott; mások rendelkezik tulajdonságokkal beállítható, hogy a virtuális gép megváltozzon jelentős.</span><span class="sxs-lookup"><span data-stu-id="314c8-106">Removing some VM Extensions is not supported; others have properties that can be set that change VM behavior radically.</span></span>
> 
> 

<span data-ttu-id="314c8-107">hello leggyakoribb feladatokat is:</span><span class="sxs-lookup"><span data-stu-id="314c8-107">hello most common tasks are:</span></span>

1. <span data-ttu-id="314c8-108">Rendelkezésre álló bővítmények keresése</span><span class="sxs-lookup"><span data-stu-id="314c8-108">Finding Available Extensions</span></span>
2. <span data-ttu-id="314c8-109">A betöltött frissítései</span><span class="sxs-lookup"><span data-stu-id="314c8-109">Updating Loaded Extensions</span></span>
3. <span data-ttu-id="314c8-110">Bővítmények hozzáadása</span><span class="sxs-lookup"><span data-stu-id="314c8-110">Adding Extensions</span></span>
4. <span data-ttu-id="314c8-111">Bővítmények eltávolítása</span><span class="sxs-lookup"><span data-stu-id="314c8-111">Removing Extensions</span></span>

## <a name="find-available-extensions"></a><span data-ttu-id="314c8-112">Rendelkezésre álló bővítmények keresése</span><span class="sxs-lookup"><span data-stu-id="314c8-112">Find Available Extensions</span></span>
<span data-ttu-id="314c8-113">Megkeresheti a bővítményt, és részletes információk segítségével:</span><span class="sxs-lookup"><span data-stu-id="314c8-113">You can locate extension and extended information using:</span></span>

* <span data-ttu-id="314c8-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="314c8-114">PowerShell</span></span>
* <span data-ttu-id="314c8-115">Az Azure platformfüggetlen parancssori felület (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="314c8-115">Azure Cross-Platform Command Line Interface (Azure CLI)</span></span>
* <span data-ttu-id="314c8-116">Szolgáltatásfelügyeleti REST API</span><span class="sxs-lookup"><span data-stu-id="314c8-116">Service Management REST API</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="314c8-117">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="314c8-117">Azure PowerShell</span></span>
<span data-ttu-id="314c8-118">Néhány bővítmény rendelkezik, amelyek adott toothem, előfordulhat, hogy könnyebben konfigurációjuk powershellből; PowerShell-parancsmagok de hello következő parancsmagok működnek az összes Virtuálisgép-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="314c8-118">Some extensions have PowerShell cmdlets that are specific toothem, which may make their configuration from PowerShell easier; but hello following cmdlets work for all VM extensions.</span></span>

<span data-ttu-id="314c8-119">A következő parancsmagok tooobtain információ a rendelkezésre álló bővítmények hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="314c8-119">You can use hello following cmdlets tooobtain information about available extensions:</span></span>

* <span data-ttu-id="314c8-120">A példányok webes szerepkörök vagy feldolgozói szerepköröket, használhatja a hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="314c8-120">For instances of web roles or worker roles, you can use hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span></span>
* <span data-ttu-id="314c8-121">A példányok a virtuális gépek, használhatja a hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="314c8-121">For instances of Virtual Machines, you can use hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span></span>
  
   <span data-ttu-id="314c8-122">Például a hogyan kód példa azt mutatja meg a következő hello toolist hello információi **IaaSDiagnostics** bővítmény PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="314c8-122">For example, hello following code example shows how toolist the information for hello **IaaSDiagnostics** extension using PowerShell.</span></span>
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a><span data-ttu-id="314c8-123">Az Azure parancssori felület (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="314c8-123">Azure Command Line Interface (Azure CLI)</span></span>
<span data-ttu-id="314c8-124">Néhány bővítmény rendelkezik, amelyek adott toothem Azure parancssori felület parancsait (Docker Virtuálisgép-bővítmény hello csak egy példa a), amely könnyebben lehet, hogy a konfigurációs; de hello következő parancsokat az összes Virtuálisgép-bővítmények számára.</span><span class="sxs-lookup"><span data-stu-id="314c8-124">Some extensions have Azure CLI commands that are specific toothem (hello Docker VM Extension is one example), which may make their configuration easier; but hello following commands work for all VM extensions.</span></span>

<span data-ttu-id="314c8-125">Használhatja a hello **azure virtuális gép Bővítménylista** tooobtain információ a rendelkezésre álló bővítmények parancsot, és a hello **–-json** toodisplay lehetőséget egy vagy több bővítmények az összes rendelkezésre álló információkat.</span><span class="sxs-lookup"><span data-stu-id="314c8-125">You can use hello **azure vm extension list** command tooobtain information about available extensions, and use hello **–-json** option toodisplay all available information about one or more extensions.</span></span> <span data-ttu-id="314c8-126">Ha nem használja a bővítmények nevének, a hello parancsot az összes rendelkezésre álló bővítmények JSON leírását adja eredményül.</span><span class="sxs-lookup"><span data-stu-id="314c8-126">If you do not use an extension name, hello command returns a JSON description of all available extensions.</span></span>

<span data-ttu-id="314c8-127">Például a következő példakód hello mutatja, hogyan toolist hello hello információi **IaaSDiagnostics** bővítmény használatával hello Azure CLI **azure virtuális gép Bővítménylista** parancs és felhasználási hello **–-json** tooreturn teljes körű információkat lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="314c8-127">For example, hello following code example shows how toolist hello information for hello **IaaSDiagnostics** extension using hello Azure CLI **azure vm extension list** command and uses hello **–-json** option tooreturn complete information.</span></span>

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a><span data-ttu-id="314c8-128">Szolgáltatáskezelési REST API-k</span><span class="sxs-lookup"><span data-stu-id="314c8-128">Service Management REST APIs</span></span>
<span data-ttu-id="314c8-129">A következő REST API-k tooobtain információ a rendelkezésre álló bővítmények hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="314c8-129">You can use hello following REST APIs tooobtain information about available extensions:</span></span>

* <span data-ttu-id="314c8-130">A példányok webes szerepkörök vagy feldolgozói szerepköröket, használhatja a hello [lista rendelkezésre álló bővítmények](https://msdn.microsoft.com/library/dn169559.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="314c8-130">For instances of web roles or worker roles, you can use hello [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span> <span data-ttu-id="314c8-131">rendelkezésre álló bővítmények toolist hello verziói esetében használhat [lista bővítmény verziók](https://msdn.microsoft.com/library/dn495437.aspx).</span><span class="sxs-lookup"><span data-stu-id="314c8-131">toolist hello versions of available extensions, you can use [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx).</span></span>
* <span data-ttu-id="314c8-132">A példányok a virtuális gépek, használhatja a hello [erőforrás-kiterjesztések listájából](https://msdn.microsoft.com/library/dn495441.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="314c8-132">For instances of Virtual Machines, you can use hello [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span> <span data-ttu-id="314c8-133">rendelkezésre álló bővítmények toolist hello verziói esetében használhat [lista erőforrás-bővítmény verziók](https://msdn.microsoft.com/library/dn495440.aspx).</span><span class="sxs-lookup"><span data-stu-id="314c8-133">toolist hello versions of available extensions, you can use [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx).</span></span>

## <a name="add-update-or-disable-extensions"></a><span data-ttu-id="314c8-134">Adja hozzá, frissítés, vagy bővítmények letiltása</span><span class="sxs-lookup"><span data-stu-id="314c8-134">Add, Update, or Disable Extensions</span></span>
<span data-ttu-id="314c8-135">Bővítmények példány létrehozásakor, illetve azok felveheti-példányt futtató tooa adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="314c8-135">Extensions can be added when an instance is created or they can be added tooa running instance.</span></span> <span data-ttu-id="314c8-136">Bővítmények frissítése, letiltott vagy eltávolított.</span><span class="sxs-lookup"><span data-stu-id="314c8-136">Extensions can be updated, disabled, or removed.</span></span> <span data-ttu-id="314c8-137">Azure PowerShell-parancsmagok használatával vagy hello szolgáltatásfelügyelet REST API műveletek segítségével ezeket a műveleteket végezheti el.</span><span class="sxs-lookup"><span data-stu-id="314c8-137">You can perform these actions by using Azure PowerShell cmdlets or by using hello Service Management REST API operations.</span></span> <span data-ttu-id="314c8-138">Paraméterek szükséges tooinstall és néhány bővítmény beállításához.</span><span class="sxs-lookup"><span data-stu-id="314c8-138">Parameters are required tooinstall and set up some extensions.</span></span> <span data-ttu-id="314c8-139">Nyilvános és titkos paraméterek a bővítmények esetében támogatottak.</span><span class="sxs-lookup"><span data-stu-id="314c8-139">Public and private parameters are supported for extensions.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="314c8-140">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="314c8-140">Azure PowerShell</span></span>
<span data-ttu-id="314c8-141">Azure PowerShell-parancsmagok használata hello legegyszerűbb módja tooadd és frissítés-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="314c8-141">Using Azure PowerShell cmdlets is hello easiest way tooadd and update extensions.</span></span> <span data-ttu-id="314c8-142">Hello bővítmény parancsmagok használatakor hello bővítmény hello-beállítások legnagyobb részét történik meg.</span><span class="sxs-lookup"><span data-stu-id="314c8-142">When you use hello extension cmdlets, most of hello configuration of hello extension is done for you.</span></span> <span data-ttu-id="314c8-143">Esetenként, esetleg tooprogrammatically veheti fel.</span><span class="sxs-lookup"><span data-stu-id="314c8-143">At times, you may need tooprogrammatically add an extension.</span></span> <span data-ttu-id="314c8-144">Ha toodo ez szükséges, meg kell adnia hello konfigurációs hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="314c8-144">When you need toodo this, you must provide hello configuration of hello extension.</span></span>

<span data-ttu-id="314c8-145">Hogy egy-bővítményhez olyan nyilvános és titkos paraméterek konfigurálása a következő parancsmagok tooknow hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="314c8-145">You can use hello following cmdlets tooknow whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="314c8-146">A példányok webes szerepkörök vagy feldolgozói szerepköröket, használhatja a hello **Get-AzureServiceAvailableExtension** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="314c8-146">For instances of web roles or worker roles, you can use hello **Get-AzureServiceAvailableExtension** cmdlet.</span></span>
* <span data-ttu-id="314c8-147">A példányok a virtuális gépek, használhatja a hello **Get-AzureVMAvailableExtension** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="314c8-147">For instances of Virtual Machines, you can use hello **Get-AzureVMAvailableExtension** cmdlet.</span></span>

### <a name="service-management-rest-apis"></a><span data-ttu-id="314c8-148">Szolgáltatáskezelési REST API-k</span><span class="sxs-lookup"><span data-stu-id="314c8-148">Service Management REST APIs</span></span>
<span data-ttu-id="314c8-149">Hello REST API-k segítségével visszaállíthatja az elérhető bővítmények listája, a kapcsolatos tudnivalókért, hogyan hello bővítmény konfigurált toobe kapja meg.</span><span class="sxs-lookup"><span data-stu-id="314c8-149">When you retrieve a listing of available extensions by using hello REST APIs, you receive information about how hello extension is toobe configured.</span></span> <span data-ttu-id="314c8-150">visszaadott hello információk megjelenítése előfordulhat, hogy egy séma nyilvános és titkos séma paraméterinformáció.</span><span class="sxs-lookup"><span data-stu-id="314c8-150">hello information that is returned might show parameter information represented by a public schema and private schema.</span></span> <span data-ttu-id="314c8-151">Visszaadja a nyilvános paraméterértékek hello példányaira vonatkozó lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="314c8-151">Public parameter values are returned in queries about hello instances.</span></span> <span data-ttu-id="314c8-152">Titkos paraméter értéke nem lehet megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="314c8-152">Private parameter values are not returned.</span></span>

<span data-ttu-id="314c8-153">E kiterjesztése a nyilvános és titkos paraméterek olyan konfigurációt igényel, a REST API-k tooknow következő hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="314c8-153">You can use hello following REST APIs tooknow whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="314c8-154">A webes szerepkörök vagy feldolgozói szerepkörök példányai hello **PublicConfigurationSchema** és **PrivateConfigurationSchema** elemek hello hello válaszára a hello információkkal [listája Rendelkezésre álló bővítmények](https://msdn.microsoft.com/library/dn169559.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="314c8-154">For instances of web roles or worker roles, hello **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain hello information in hello response from hello [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span>
* <span data-ttu-id="314c8-155">A virtuális gépek példányait hello **PublicConfigurationSchema** és **PrivateConfigurationSchema** elemek hello hello válaszára a hello információkkal [listája Erőforrás-bővítményt](https://msdn.microsoft.com/library/dn495441.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="314c8-155">For instances of Virtual Machines, hello **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain hello information in hello response from hello [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span>

> [!NOTE]
> <span data-ttu-id="314c8-156">Bővítmények JSON meghatározott konfigurációk is használható.</span><span class="sxs-lookup"><span data-stu-id="314c8-156">Extensions can also use configurations that are defined with JSON.</span></span> <span data-ttu-id="314c8-157">Az ilyen típusú kiterjesztések használata esetén csak hello **SampleConfig** elem szolgál.</span><span class="sxs-lookup"><span data-stu-id="314c8-157">When these types of extensions are used, only hello **SampleConfig** element is used.</span></span>
> 
> 

