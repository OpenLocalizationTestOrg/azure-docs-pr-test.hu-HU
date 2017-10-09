
<span data-ttu-id="75370-101">Microsoft Azure-felhőszolgáltatásban problémák diagnosztizálása szükséges virtuális gépeken hello szolgáltatás-naplófájl összegyűjtése a hello hibák bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="75370-101">Diagnosing issues with an Microsoft Azure cloud service requires collecting hello service’s log files on virtual machines as hello issues occur.</span></span> <span data-ttu-id="75370-102">Hello AzureLogCollector bővítmény igény szerinti tooperfom egyszeri gyűjtemény naplók egy vagy több Cloud Service virtuális gépeken (a egyaránt webes és feldolgozói szerepkörök), és átviteli hello gyűjtött fájlok tooan Azure storage-fiókok – az összes távoli bejelentkezés nélkül is használhatja a virtuális gépek hello tooany.</span><span class="sxs-lookup"><span data-stu-id="75370-102">You can use hello AzureLogCollector extension on-demand tooperfom one-time collection of logs from one or more Cloud Service VMs (from both web roles and worker roles) and transfer hello collected files tooan Azure storage account – all without remotely logging on tooany of hello VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="75370-103">A legtöbb hello naplózott adatok leírása http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp helyen találhatók meg.</span><span class="sxs-lookup"><span data-stu-id="75370-103">Descriptions for most of hello logged information can be found at http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.</span></span>
> 
> 

<span data-ttu-id="75370-104">Gyűjtemény két módja van hello típusú összegyűjtött fájlok toobe függ.</span><span class="sxs-lookup"><span data-stu-id="75370-104">There are two modes of collection dependent on hello types of files toobe collected.</span></span>

* <span data-ttu-id="75370-105">Az Azure Vendég ügynök naplók csak (GA).</span><span class="sxs-lookup"><span data-stu-id="75370-105">Azure Guest Agent Logs only (GA).</span></span> <span data-ttu-id="75370-106">A gyűjtemény üzemmódja összes hello naplók kapcsolódó tooAzure vendégügynökök és egyéb Azure összetevőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="75370-106">This collection mode includes all hello logs related tooAzure guest agents and other Azure components.</span></span>
* <span data-ttu-id="75370-107">Összes napló (teljes).</span><span class="sxs-lookup"><span data-stu-id="75370-107">All Logs (Full).</span></span> <span data-ttu-id="75370-108">A gyűjtemény üzemmódja összegyűjti az összes fájl plusz GA módban:</span><span class="sxs-lookup"><span data-stu-id="75370-108">This collection mode will collect all files in GA mode plus:</span></span>
  
  * <span data-ttu-id="75370-109">rendszer- és eseménynaplók</span><span class="sxs-lookup"><span data-stu-id="75370-109">system and application event logs</span></span>
  * <span data-ttu-id="75370-110">A HTTP hibanaplókat.</span><span class="sxs-lookup"><span data-stu-id="75370-110">HTTP error logs</span></span>
  * <span data-ttu-id="75370-111">IIS-napló</span><span class="sxs-lookup"><span data-stu-id="75370-111">IIS Logs</span></span>
  * <span data-ttu-id="75370-112">Telepítési naplók</span><span class="sxs-lookup"><span data-stu-id="75370-112">Setup logs</span></span>
  * <span data-ttu-id="75370-113">egyéb rendszer naplóit</span><span class="sxs-lookup"><span data-stu-id="75370-113">other system logs</span></span>

<span data-ttu-id="75370-114">Mindkét gyűjtemény módban további adatok gyűjteménymappához struktúra a következő hello gyűjteménye használatával adhatók meg:</span><span class="sxs-lookup"><span data-stu-id="75370-114">In both collection modes, additional data collection folders can be specified by using a collection of hello following structure:</span></span>

* <span data-ttu-id="75370-115">**Név**: hello hello gyűjtemény nevét, hello neveként almappa hello zip-fájl toobe belül használt gyűjti.</span><span class="sxs-lookup"><span data-stu-id="75370-115">**Name**: hello name of hello collection, which will be used as hello name of subfolder inside hello zip file toobe collected.</span></span>
* <span data-ttu-id="75370-116">**Hely**: hello elérési toohello mappa hello virtuális gépen fájl hova legyenek összegyűjtve.</span><span class="sxs-lookup"><span data-stu-id="75370-116">**Location**: hello path toohello folder on hello virtual machine where file will be collected.</span></span>
* <span data-ttu-id="75370-117">**SearchPattern**: gyűjtött fájlok toobe hello nevei hello mintáját.</span><span class="sxs-lookup"><span data-stu-id="75370-117">**SearchPattern**: hello pattern of hello names of files toobe collected.</span></span> <span data-ttu-id="75370-118">Alapértelmezett érték a "*"</span><span class="sxs-lookup"><span data-stu-id="75370-118">Default is “*”</span></span>
* <span data-ttu-id="75370-119">**Rekurzív**: Ha hello fájlok lesznek hello mappa alatt gyűjtött rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="75370-119">**Recursive**: if hello files will be collected recursively under hello folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75370-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="75370-120">Prerequisites</span></span>
* <span data-ttu-id="75370-121">Generált toosave zip bővítményfájlok toohave tárfiók van szükség.</span><span class="sxs-lookup"><span data-stu-id="75370-121">You need toohave a storage account for extension toosave generated zip files.</span></span>
* <span data-ttu-id="75370-122">Meg kell győződnie arról, hogy azok be Azure PowerShell-parancsmagok V0.8.0 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="75370-122">You must make sure that you are using Azure PowerShell Cmdlets V0.8.0 or above.</span></span> <span data-ttu-id="75370-123">További információkért lásd: [Azure letölti](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="75370-123">For more information, see [Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>

## <a name="add-hello-extension"></a><span data-ttu-id="75370-124">Hello-bővítmény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="75370-124">Add hello extension</span></span>
<span data-ttu-id="75370-125">Használhat [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) parancsmagok vagy [Service Management REST API-k](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector bővítmény.</span><span class="sxs-lookup"><span data-stu-id="75370-125">You can use [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets or [Service Management REST APIs](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector extension.</span></span>

<span data-ttu-id="75370-126">A Felhőszolgáltatások hello a meglévő Azure Powershell-parancsmag **Set-AzureServiceExtension**, felhőalapú szolgáltatás szerepkörpéldányokat használt tooenable hello kiterjesztés lehet.</span><span class="sxs-lookup"><span data-stu-id="75370-126">For Cloud Services, hello existing Azure Powershell cmdlet, **Set-AzureServiceExtension**, can be used tooenable hello extension on Cloud Service role instances.</span></span> <span data-ttu-id="75370-127">Minden alkalommal, amikor a bővítmény engedélyezve van ez a parancsmag használatával, naplógyűjtést akkor váltódik ki, a kiválasztott szerepkörök hello kijelölt szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="75370-127">Every time this extension is enabled through this cmdlet, log collection is triggered on hello selected role instances of selected roles.</span></span>

<span data-ttu-id="75370-128">A virtuális gépek, a meglévő Azure Powershell-parancsmag hello **Set-AzureVMExtension**, használt tooenable hello bővítményt a virtuális gépeken is lehet.</span><span class="sxs-lookup"><span data-stu-id="75370-128">For Virtual Machines, hello existing Azure Powershell cmdlet, **Set-AzureVMExtension**, can be used tooenable hello extension on Virtual Machines.</span></span> <span data-ttu-id="75370-129">Minden alkalommal, amikor a bővítmény engedélyezve van a hello parancsmagokon keresztül, az egyes példányok naplógyűjtést váltja ki.</span><span class="sxs-lookup"><span data-stu-id="75370-129">Every time this extension is enabled through hello cmdlets, log collection is triggered on each instance.</span></span>

<span data-ttu-id="75370-130">Ehhez a kiterjesztéshez belső JSON-alapú PublicConfiguration hello és PrivateConfiguration használja.</span><span class="sxs-lookup"><span data-stu-id="75370-130">Internally, this extension uses hello JSON-based PublicConfiguration and PrivateConfiguration.</span></span> <span data-ttu-id="75370-131">hello hello elrendezés egy minta JSON nyilvános és titkos konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="75370-131">hello following is hello layout of a sample JSON for public and private configuration.</span></span>

### <a name="publicconfiguration"></a><span data-ttu-id="75370-132">PublicConfiguration</span><span class="sxs-lookup"><span data-stu-id="75370-132">PublicConfiguration</span></span>
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri tooyour storage account with sp=wl",
        "AdditionalData":
        [
          {
                  "Name":  "StorageData",
                  "Location":  "%roleroot%storage",
                  "SearchPattern":  "*.*",
                  "Recursive":  "true"
          },
          {
                "Name":  "CustomDataFolder2",
                "Location":  "c:\customFolder",
                "SearchPattern":  "*.log",
                "Recursive":  "false"
          },
        ]
    }

### <a name="privateconfiguration"></a><span data-ttu-id="75370-133">PrivateConfiguration</span><span class="sxs-lookup"><span data-stu-id="75370-133">PrivateConfiguration</span></span>
    {

    }

> [!NOTE]
> <span data-ttu-id="75370-134">Ehhez a bővítményhez nem szükséges **privateConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="75370-134">This extension doesn’t need **privateConfiguration**.</span></span> <span data-ttu-id="75370-135">Csak egy üres struktúra biztosíthatja hello **– PrivateConfiguration** argumentum.</span><span class="sxs-lookup"><span data-stu-id="75370-135">You can just provide an empty structure for hello **–PrivateConfiguration** argument.</span></span>
> 
> 

<span data-ttu-id="75370-136">Hello két kövesse a következő lépéseket tooadd hello AzureLogCollector tooone vagy több példány egy felhőalapú szolgáltatás vagy a virtuális gép a kiválasztott szerepkörök, mely eseményindítók minden virtuális gép toorun gyűjteményeit hello és tooAzure fiók hello gyűjtött fájlok küldése a megadott.</span><span class="sxs-lookup"><span data-stu-id="75370-136">You can follow one of hello two following steps tooadd hello AzureLogCollector tooone or more instances of a Cloud Service or Virtual Machine of selected roles, which triggers hello collections on each VM toorun and send hello collected files tooAzure account specified.</span></span>

## <a name="adding-as-a-service-extension"></a><span data-ttu-id="75370-137">Egy bővítmény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="75370-137">Adding as a Service Extension</span></span>
1. <span data-ttu-id="75370-138">Hajtsa végre a hello utasításokat tooconnect Azure PowerShell tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="75370-138">Follow hello instructions tooconnect Azure PowerShell tooyour subscription.</span></span>
2. <span data-ttu-id="75370-139">Adja meg a hello szolgáltatás neve, a tárhely, a szerepkörök és a szerepkör példányok toowhich szeretné, hogy tooadd és hello AzureLogCollector bővítmény engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="75370-139">Specify hello service name, slot, roles, and role instances toowhich you want tooadd and enable hello AzureLogCollector extension.</span></span>
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify hello slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified hello roles on which hello extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify hello instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
3. <span data-ttu-id="75370-140">Adja meg a fájlok gyűjtenek hello további adatok mappa (Ez a lépés nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="75370-140">Specify hello additional data folder for which files will be collected (this step is optional).</span></span>
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > <span data-ttu-id="75370-141">Használhatja a token `%roleroot%` toospecify hello szerepkör gyökérmeghajtóján óta nem használ rögzített meghajtóra.</span><span class="sxs-lookup"><span data-stu-id="75370-141">You can use token `%roleroot%` toospecify hello role root drive since it doesn’t use a fixed drive.</span></span>
   > 
   > 
4. <span data-ttu-id="75370-142">Adja meg a hello az Azure storage-fiók nevét és a kulcs toowhich gyűjtött fájlok lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="75370-142">Provide hello Azure storage account name and key toowhich collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. <span data-ttu-id="75370-143">Hello (hello hello cikk végén található) SetAzureServiceLogCollector.ps1 követi tooenable hello AzureLogCollector bővítményként kérjen egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="75370-143">Call hello SetAzureServiceLogCollector.ps1 (included at hello end of hello article) as follows tooenable hello AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="75370-144">Ha hello végrehajtása befejeződött, a hello feltöltött fájl található`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span><span class="sxs-lookup"><span data-stu-id="75370-144">Once hello execution is completed, you can find hello uploaded file under `https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span></span>
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

<span data-ttu-id="75370-145">hello hello átadott toohello parancsfájl hello definíciója látható.</span><span class="sxs-lookup"><span data-stu-id="75370-145">hello following is hello definition of hello parameters passed toohello script.</span></span> <span data-ttu-id="75370-146">(Ez másolódik alatt is.)</span><span class="sxs-lookup"><span data-stu-id="75370-146">(This is copied below as well.)</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
      [Parameter(Mandatory=$true)]
    [string]   $ServiceName,

    [Parameter(Mandatory=$false)]
    [string[]] $Roles ,

    [Parameter(Mandatory=$false)]
    [string[]] $Instances,

    [Parameter(Mandatory=$false)]
    [string]   $Slot = 'Production',

    [Parameter(Mandatory=$false)]
    [string]   $Mode = 'Full',

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountName,

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountKey,

    [Parameter(Mandatory=$false)]
    [PSObject[]] $AdditionDataLocationList = $null
    )

* <span data-ttu-id="75370-147">*Szolgáltatásnév*: A felhőszolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="75370-147">*ServiceName*: Your cloud service name.</span></span>
* <span data-ttu-id="75370-148">*Szerepkörök*: a szerepköröket, például "WebRole1" vagy "WorkerRole1" listáját.</span><span class="sxs-lookup"><span data-stu-id="75370-148">*Roles*: A list of roles, such as “WebRole1” or ”WorkerRole1”.</span></span>
* <span data-ttu-id="75370-149">*Példányok*: hello nevei a szerepkörpéldányok vesszővel elválasztott – hello helyettesítő karakterlánc ("*") tartozó összes szerepkörpéldányt.</span><span class="sxs-lookup"><span data-stu-id="75370-149">*Instances*: A list of hello names of role instances separated by comma -- use hello wildcard string (“*”) for all role instances.</span></span>
* <span data-ttu-id="75370-150">*Tárolóhely*: tárhely neve.</span><span class="sxs-lookup"><span data-stu-id="75370-150">*Slot*: Slot name.</span></span> <span data-ttu-id="75370-151">"Éles" vagy "Tesztelés".</span><span class="sxs-lookup"><span data-stu-id="75370-151">“Production” or “Staging”.</span></span>
* <span data-ttu-id="75370-152">*Mód*: gyűjtemény módja.</span><span class="sxs-lookup"><span data-stu-id="75370-152">*Mode*: Collection mode.</span></span> <span data-ttu-id="75370-153">"Teljes" vagy "GA".</span><span class="sxs-lookup"><span data-stu-id="75370-153">“Full” or “GA”.</span></span>
* <span data-ttu-id="75370-154">*StorageAccountName*: neve az Azure storage-fiók tárolására összegyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="75370-154">*StorageAccountName*: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="75370-155">*StorageAccountKey*: neve az Azure tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="75370-155">*StorageAccountKey*: Name of Azure storage account key.</span></span>
* <span data-ttu-id="75370-156">*AdditionalDataLocationList*: a következő struktúra hello listáját:</span><span class="sxs-lookup"><span data-stu-id="75370-156">*AdditionalDataLocationList*: A list of hello following structure:</span></span>
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a><span data-ttu-id="75370-157">A Virtuálisgép-bővítmény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="75370-157">Adding as a VM Extension</span></span>
<span data-ttu-id="75370-158">Hajtsa végre a hello utasításokat tooconnect Azure PowerShell tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="75370-158">Follow hello instructions tooconnect Azure PowerShell tooyour subscription.</span></span>

1. <span data-ttu-id="75370-159">Adja meg a hello szolgáltatás neve, a virtuális gép és a hello gyűjtemény módja.</span><span class="sxs-lookup"><span data-stu-id="75370-159">Specify hello service name, VM, and hello collection mode.</span></span>
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify hello VM name
        $VMName = "'YourVMName'"
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify hello additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. <span data-ttu-id="75370-160">Adja meg a hello az Azure storage-fiók nevét és a kulcs toowhich gyűjtött fájlok lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="75370-160">Provide hello Azure storage account name and key toowhich collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. <span data-ttu-id="75370-161">Hello (hello hello cikk végén található) SetAzureVMLogCollector.ps1 követi tooenable hello AzureLogCollector bővítményként kérjen egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="75370-161">Call hello SetAzureVMLogCollector.ps1 (included at hello end of hello article) as follows tooenable hello AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="75370-162">Ha hello végrehajtása befejeződött, a https://YouareStorageAccountName.blob.core.windows.net/vmlogs hello feltöltött fájl található</span><span class="sxs-lookup"><span data-stu-id="75370-162">Once hello execution is completed, you can find hello uploaded file under https://YouareStorageAccountName.blob.core.windows.net/vmlogs</span></span>

<span data-ttu-id="75370-163">hello hello átadott toohello parancsfájl hello definíciója látható.</span><span class="sxs-lookup"><span data-stu-id="75370-163">hello following is hello definition of hello parameters passed toohello script.</span></span> <span data-ttu-id="75370-164">(Ez másolódik alatt is.)</span><span class="sxs-lookup"><span data-stu-id="75370-164">(This is copied below as well.)</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
        [Parameter(Mandatory=$true)]
      [string]   $ServiceName,

      [Parameter(Mandatory=$false)]
      [string] $VMName ,

        [Parameter(Mandatory=$false)]
      [string]   $Mode = 'Full',

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountName,

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountKey,

      [Parameter(Mandatory=$false)]
      [PSObject[]] $AdditionDataLocationList = $null
      )

* <span data-ttu-id="75370-165">Szolgáltatásnév: A felhőszolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="75370-165">ServiceName: Your cloud service name.</span></span>
* <span data-ttu-id="75370-166">Virtuális gép hello VMName hello neve.</span><span class="sxs-lookup"><span data-stu-id="75370-166">VMName hello name of hello VM.</span></span>
* <span data-ttu-id="75370-167">Mód: Gyűjtemény mód.</span><span class="sxs-lookup"><span data-stu-id="75370-167">Mode: Collection mode.</span></span> <span data-ttu-id="75370-168">"Teljes" vagy "GA".</span><span class="sxs-lookup"><span data-stu-id="75370-168">“Full” or “GA”.</span></span>
* <span data-ttu-id="75370-169">StorageAccountName: A gyűjtött adatok tárolásához Azure storage-fiókjának neve.</span><span class="sxs-lookup"><span data-stu-id="75370-169">StorageAccountName: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="75370-170">StorageAccountKey: Azure storage-fiók kulcs neve.</span><span class="sxs-lookup"><span data-stu-id="75370-170">StorageAccountKey: Name of Azure storage account key.</span></span>
* <span data-ttu-id="75370-171">AdditionalDataLocationList: Hello struktúra a következő listája:</span><span class="sxs-lookup"><span data-stu-id="75370-171">AdditionalDataLocationList: A list of hello following structure:</span></span>

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a><span data-ttu-id="75370-172">Kiterjesztés PowerShell parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="75370-172">Extention PowerShell Script files</span></span>
<span data-ttu-id="75370-173">SetAzureServiceLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="75370-173">SetAzureServiceLogCollector.ps1</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Roles ,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Instances = '*',

                  [Parameter(Mandatory=$false)]
                  [string]   $Slot = 'Production',

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
    {
        $instanceText = $Instances[0]
        for ($i = 1;$i -lt $Instances.Count;$i++)
        {
              $instanceText = $instanceText+ "," + $Instances[$i]
          }
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
    }
    else  #For all instances if not specified.  hello value should be a space or *
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
    }

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json
    "publicConfig is:  $publicConfigJSON"

    #we just provide a empty privateConfig object
    $privateconfig = "{
    }"

    if ($Roles -ne $null)
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }
    else
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }

    #
    #This is an optional step: generate a sasUri toohello container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "hello container for uploaded file can be accessed using this link:`r`n$sasuri"


<span data-ttu-id="75370-174">SetAzureVMLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="75370-174">SetAzureVMLogCollector.ps1</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string] $VMName ,

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
    #
    $privateconfig = "{
    }"

    if ($VMName -ne $null )
    {
          $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
          $VM.VM.OSVirtualHardDisk.OS

          if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
          {
                Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

                #
                #We will check hello VM status toofind if operation by extension has been completed or not. hello completion of hello operation,either succeed or fail, can be indicated by
                #hello presence of SubstatusList field.
                #
                $Completed = $false
                while ($Completed -ne $true)
                {
                        $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                        $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                        if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                        {
                            Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                              $Completed = $true
                        }
                        elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                        {
                              $Completed = $false
                              Write-Output "Waiting for operation toocomplete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access toohello file, we can generate a read-only SasUri directly toohello file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "hello uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove hello extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, hello extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, hello extension cannot be enabled"
    }

## <a name="next-steps"></a><span data-ttu-id="75370-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="75370-175">Next Steps</span></span>
<span data-ttu-id="75370-176">Most vizsgálja meg, vagy a naplók másolása egy nagyon egyszerű helyről.</span><span class="sxs-lookup"><span data-stu-id="75370-176">Now you can examine or copy your logs from one very simple location.</span></span>

