


## <a name="using-vm-extensions"></a><span data-ttu-id="50692-101">Virtuálisgép-bővítmények használatával</span><span class="sxs-lookup"><span data-stu-id="50692-101">Using VM Extensions</span></span>
<span data-ttu-id="50692-102">Az Azure Virtuálisgép-bővítmények megvalósítása viselkedések vagy szolgáltatásokat vagy más programok Azure virtuális gépeken használata (például a **WebDeployForVSDevTest** bővítmény lehetővé teszi, hogy a Visual Studio Web Deploy megoldásokhoz az Azure virtuális gépre), vagy adjon meg a Ahhoz, hogy a virtuális gép kommunikálni képes néhány más viselkedés támogatására (például segítségével a virtuális gép eléréséhez kapcsolódó kiegészítő mezőket PowerShell, az Azure CLI és a többi ügyféltől alaphelyzetbe állítása, vagy módosítsa a távelérés értékek az Azure virtuális gép).</span><span class="sxs-lookup"><span data-stu-id="50692-102">Azure VM Extensions implement behaviors or features that either help other programs work on Azure VMs (for example, the **WebDeployForVSDevTest** extension allows Visual Studio to Web Deploy solutions on your Azure VM) or provide the ability for you to interact with the VM to support some other behavior (for example, you can use the VM Access extensions from PowerShell, the Azure CLI, and REST clients to reset or modify remote access values on your Azure VM).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50692-103">Bővítmények támogatják-e a szolgáltatások által teljes listáját lásd: [Azure Virtuálisgép-bővítmények és a szolgáltatások](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="50692-103">For a complete list of extensions by the features they support, see [Azure VM Extensions and Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="50692-104">Mivel minden egyes Virtuálisgép-bővítmény egy adott funkcióhoz támogatja, pontosan mit, illetve használhat nem kiterjesztési függ a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="50692-104">Because each VM extension supports a specific feature, exactly what you can and cannot do with an extension depends on the extension.</span></span> <span data-ttu-id="50692-105">-Módosítása előtt a virtuális gép győződjön meg arról, hogy elolvasta a használni kívánt Virtuálisgép-bővítmény dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="50692-105">Therefore, before modifying your VM, make sure you have read the documentation for the VM Extension you want to use.</span></span> <span data-ttu-id="50692-106">Egyes Virtuálisgép-bővítmények nem támogatott; mások rendelkezik tulajdonságokkal beállítható, hogy a virtuális gép megváltozzon jelentős.</span><span class="sxs-lookup"><span data-stu-id="50692-106">Removing some VM Extensions is not supported; others have properties that can be set that change VM behavior radically.</span></span>
> 
> 

<span data-ttu-id="50692-107">A leggyakoribb feladatokat is:</span><span class="sxs-lookup"><span data-stu-id="50692-107">The most common tasks are:</span></span>

1. <span data-ttu-id="50692-108">Rendelkezésre álló bővítmények keresése</span><span class="sxs-lookup"><span data-stu-id="50692-108">Finding Available Extensions</span></span>
2. <span data-ttu-id="50692-109">A betöltött frissítései</span><span class="sxs-lookup"><span data-stu-id="50692-109">Updating Loaded Extensions</span></span>
3. <span data-ttu-id="50692-110">Bővítmények hozzáadása</span><span class="sxs-lookup"><span data-stu-id="50692-110">Adding Extensions</span></span>
4. <span data-ttu-id="50692-111">Bővítmények eltávolítása</span><span class="sxs-lookup"><span data-stu-id="50692-111">Removing Extensions</span></span>

## <a name="find-available-extensions"></a><span data-ttu-id="50692-112">Rendelkezésre álló bővítmények keresése</span><span class="sxs-lookup"><span data-stu-id="50692-112">Find Available Extensions</span></span>
<span data-ttu-id="50692-113">Megkeresheti a bővítményt, és részletes információk segítségével:</span><span class="sxs-lookup"><span data-stu-id="50692-113">You can locate extension and extended information using:</span></span>

* <span data-ttu-id="50692-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50692-114">PowerShell</span></span>
* <span data-ttu-id="50692-115">Az Azure platformfüggetlen parancssori felület (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="50692-115">Azure Cross-Platform Command Line Interface (Azure CLI)</span></span>
* <span data-ttu-id="50692-116">Szolgáltatásfelügyeleti REST API</span><span class="sxs-lookup"><span data-stu-id="50692-116">Service Management REST API</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="50692-117">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="50692-117">Azure PowerShell</span></span>
<span data-ttu-id="50692-118">Néhány bővítmény rendelkezik PowerShell-parancsmagok, amelyek adott ki, amely előfordulhat, hogy könnyebben konfigurációjuk powershellből; de a következő parancsmag az összes Virtuálisgép-bővítmények fog működni.</span><span class="sxs-lookup"><span data-stu-id="50692-118">Some extensions have PowerShell cmdlets that are specific to them, which may make their configuration from PowerShell easier; but the following cmdlets work for all VM extensions.</span></span>

<span data-ttu-id="50692-119">Az alábbi parancsmagok segítségével rendelkezésre álló bővítmények kapcsolatos információkhoz:</span><span class="sxs-lookup"><span data-stu-id="50692-119">You can use the following cmdlets to obtain information about available extensions:</span></span>

* <span data-ttu-id="50692-120">A webes szerepkörök vagy feldolgozói szerepkörök példányai, használhatja a [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="50692-120">For instances of web roles or worker roles, you can use the [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span></span>
* <span data-ttu-id="50692-121">A virtuális gépek példányai, használhatja a [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="50692-121">For instances of Virtual Machines, you can use the [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span></span>
  
   <span data-ttu-id="50692-122">Például az alábbi példakód bemutatja, hogyan-információinak felsorolásához a **IaaSDiagnostics** bővítmény PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="50692-122">For example, the following code example shows how to list the information for the **IaaSDiagnostics** extension using PowerShell.</span></span>
  
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

### <a name="azure-command-line-interface-azure-cli"></a><span data-ttu-id="50692-123">Az Azure parancssori felület (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="50692-123">Azure Command Line Interface (Azure CLI)</span></span>
<span data-ttu-id="50692-124">Néhány bővítmény van rájuk vonatkozó Azure parancssori felület parancsait (a Docker Virtuálisgép-bővítmény csak egy példa a), amely könnyebben lehet, hogy a konfigurációs; de a következő parancsokat az összes Virtuálisgép-bővítmények fog működni.</span><span class="sxs-lookup"><span data-stu-id="50692-124">Some extensions have Azure CLI commands that are specific to them (the Docker VM Extension is one example), which may make their configuration easier; but the following commands work for all VM extensions.</span></span>

<span data-ttu-id="50692-125">Használhatja a **azure virtuális gép Bővítménylista** parancsot a rendelkezésre álló bővítmények kapcsolatos információkhoz, és használja a **–-json** lehetőséget az összes rendelkezésre álló információkat egy vagy több bővítmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="50692-125">You can use the **azure vm extension list** command to obtain information about available extensions, and use the **–-json** option to display all available information about one or more extensions.</span></span> <span data-ttu-id="50692-126">Ha nem használja a bővítmények nevének, a parancs az összes rendelkezésre álló bővítmények JSON leírását adja meg.</span><span class="sxs-lookup"><span data-stu-id="50692-126">If you do not use an extension name, the command returns a JSON description of all available extensions.</span></span>

<span data-ttu-id="50692-127">Például az alábbi példakód bemutatja, hogyan-információinak felsorolásához a **IaaSDiagnostics** az Azure parancssori felület használatával bővítmény **azure virtuális gép Bővítménylista** parancs, és használja a **–-json**  teljes információt lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="50692-127">For example, the following code example shows how to list the information for the **IaaSDiagnostics** extension using the Azure CLI **azure vm extension list** command and uses the **–-json** option to return complete information.</span></span>

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



### <a name="service-management-rest-apis"></a><span data-ttu-id="50692-128">Szolgáltatáskezelési REST API-k</span><span class="sxs-lookup"><span data-stu-id="50692-128">Service Management REST APIs</span></span>
<span data-ttu-id="50692-129">A következő REST API-k segítségével rendelkezésre álló bővítmények kapcsolatos információkhoz:</span><span class="sxs-lookup"><span data-stu-id="50692-129">You can use the following REST APIs to obtain information about available extensions:</span></span>

* <span data-ttu-id="50692-130">A webes szerepkörök vagy feldolgozói szerepkörök példányai, használhatja a [lista rendelkezésre álló bővítmények](https://msdn.microsoft.com/library/dn169559.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="50692-130">For instances of web roles or worker roles, you can use the [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span> <span data-ttu-id="50692-131">Rendelkezésre álló bővítmények verzióinak listázása segítségével [lista bővítmény verziók](https://msdn.microsoft.com/library/dn495437.aspx).</span><span class="sxs-lookup"><span data-stu-id="50692-131">To list the versions of available extensions, you can use [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx).</span></span>
* <span data-ttu-id="50692-132">A virtuális gépek példányai, használhatja a [erőforrás-kiterjesztések listájából](https://msdn.microsoft.com/library/dn495441.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="50692-132">For instances of Virtual Machines, you can use the [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span> <span data-ttu-id="50692-133">Rendelkezésre álló bővítmények verzióinak listázása segítségével [lista erőforrás-bővítmény verziók](https://msdn.microsoft.com/library/dn495440.aspx).</span><span class="sxs-lookup"><span data-stu-id="50692-133">To list the versions of available extensions, you can use [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx).</span></span>

## <a name="add-update-or-disable-extensions"></a><span data-ttu-id="50692-134">Adja hozzá, frissítés, vagy bővítmények letiltása</span><span class="sxs-lookup"><span data-stu-id="50692-134">Add, Update, or Disable Extensions</span></span>
<span data-ttu-id="50692-135">Bővítmények példány létrehozásakor, illetve azok lehet hozzáadni egy futó példány lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="50692-135">Extensions can be added when an instance is created or they can be added to a running instance.</span></span> <span data-ttu-id="50692-136">Bővítmények frissítése, letiltott vagy eltávolított.</span><span class="sxs-lookup"><span data-stu-id="50692-136">Extensions can be updated, disabled, or removed.</span></span> <span data-ttu-id="50692-137">Azure PowerShell-parancsmagokkal vagy a Service Management REST API-műveletek segítségével ezeket a műveleteket végezheti el.</span><span class="sxs-lookup"><span data-stu-id="50692-137">You can perform these actions by using Azure PowerShell cmdlets or by using the Service Management REST API operations.</span></span> <span data-ttu-id="50692-138">Paraméterek telepítése és beállítása az egyes bővítmények szükségesek.</span><span class="sxs-lookup"><span data-stu-id="50692-138">Parameters are required to install and set up some extensions.</span></span> <span data-ttu-id="50692-139">Nyilvános és titkos paraméterek a bővítmények esetében támogatottak.</span><span class="sxs-lookup"><span data-stu-id="50692-139">Public and private parameters are supported for extensions.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="50692-140">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="50692-140">Azure PowerShell</span></span>
<span data-ttu-id="50692-141">Azure PowerShell-parancsmagok használatával hozzá és frissítsenek bővítmények legegyszerűbb módja.</span><span class="sxs-lookup"><span data-stu-id="50692-141">Using Azure PowerShell cmdlets is the easiest way to add and update extensions.</span></span> <span data-ttu-id="50692-142">A bővítmény-parancsmagok használata esetén a bővítmény konfigurációja a legtöbb történik meg.</span><span class="sxs-lookup"><span data-stu-id="50692-142">When you use the extension cmdlets, most of the configuration of the extension is done for you.</span></span> <span data-ttu-id="50692-143">Esetenként esetleg programozott módon veheti fel.</span><span class="sxs-lookup"><span data-stu-id="50692-143">At times, you may need to programmatically add an extension.</span></span> <span data-ttu-id="50692-144">Amikor erre szükség, meg kell adni a bővítmény konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="50692-144">When you need to do this, you must provide the configuration of the extension.</span></span>

<span data-ttu-id="50692-145">A következő parancsmag segítségével tudja, hogy egy bővítmény igényel-e a nyilvános és titkos paraméterek konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="50692-145">You can use the following cmdlets to know whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="50692-146">A webes szerepkörök vagy feldolgozói szerepkörök példányai, használhatja a **Get-AzureServiceAvailableExtension** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="50692-146">For instances of web roles or worker roles, you can use the **Get-AzureServiceAvailableExtension** cmdlet.</span></span>
* <span data-ttu-id="50692-147">A virtuális gépek példányai, használhatja a **Get-AzureVMAvailableExtension** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="50692-147">For instances of Virtual Machines, you can use the **Get-AzureVMAvailableExtension** cmdlet.</span></span>

### <a name="service-management-rest-apis"></a><span data-ttu-id="50692-148">Szolgáltatáskezelési REST API-k</span><span class="sxs-lookup"><span data-stu-id="50692-148">Service Management REST APIs</span></span>
<span data-ttu-id="50692-149">Amikor a rendelkezésre álló bővítmények lista tartalmazza a REST API-k használatával, úgy kell konfigurálni, a bővítmény Mitől információt kap.</span><span class="sxs-lookup"><span data-stu-id="50692-149">When you retrieve a listing of available extensions by using the REST APIs, you receive information about how the extension is to be configured.</span></span> <span data-ttu-id="50692-150">A visszaadott információk megjelenítése előfordulhat, hogy egy séma nyilvános és titkos séma paraméterinformáció.</span><span class="sxs-lookup"><span data-stu-id="50692-150">The information that is returned might show parameter information represented by a public schema and private schema.</span></span> <span data-ttu-id="50692-151">Visszaadja a nyilvános paraméterértékeket a példányaira vonatkozó lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="50692-151">Public parameter values are returned in queries about the instances.</span></span> <span data-ttu-id="50692-152">Titkos paraméter értéke nem lehet megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="50692-152">Private parameter values are not returned.</span></span>

<span data-ttu-id="50692-153">A következő REST API-k segítségével tudja, hogy egy bővítmény igényel-e a nyilvános és titkos paraméterek konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="50692-153">You can use the following REST APIs to know whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="50692-154">A webes szerepkörök vagy feldolgozói szerepköröket, a **PublicConfigurationSchema** és **PrivateConfigurationSchema** elemek információkkal a válaszát a [lista érhető el Bővítmények](https://msdn.microsoft.com/library/dn169559.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="50692-154">For instances of web roles or worker roles, the **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain the information in the response from the [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span>
* <span data-ttu-id="50692-155">A virtuális gépek példányait a **PublicConfigurationSchema** és **PrivateConfigurationSchema** elemek információkkal a válaszát a [lista erőforrás Bővítmények](https://msdn.microsoft.com/library/dn495441.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="50692-155">For instances of Virtual Machines, the **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain the information in the response from the [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span>

> [!NOTE]
> <span data-ttu-id="50692-156">Bővítmények JSON meghatározott konfigurációk is használható.</span><span class="sxs-lookup"><span data-stu-id="50692-156">Extensions can also use configurations that are defined with JSON.</span></span> <span data-ttu-id="50692-157">Ha ilyen típusú kiterjesztések csak akkor használja, a **SampleConfig** elem szolgál.</span><span class="sxs-lookup"><span data-stu-id="50692-157">When these types of extensions are used, only the **SampleConfig** element is used.</span></span>
> 
> 

