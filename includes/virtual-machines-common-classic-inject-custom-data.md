


<span data-ttu-id="09888-101">Ez a témakör ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="09888-101">This topic describes how to:</span></span>

* <span data-ttu-id="09888-102">Szúrjon be egy Azure virtuális gép (VM) adatokat, amikor folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="09888-102">Inject data into an Azure virtual machine (VM) when it is being provisioned.</span></span>
* <span data-ttu-id="09888-103">Beolvasni a Windows és Linux.</span><span class="sxs-lookup"><span data-stu-id="09888-103">Retrieve it for both Windows and Linux.</span></span>
* <span data-ttu-id="09888-104">Egyes rendszerek különleges elérhető eszközök segítségével észleli és automatikusan kezelni az egyéni adatokat.</span><span class="sxs-lookup"><span data-stu-id="09888-104">Use special tools available on some systems to detect and handle custom data automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="09888-105">Ez a cikk ismerteti, hogyan egyéni adatok beszúrhatja egy virtuális Gépet létrehozni az Azure szolgáltatásfelügyeleti API használatával.</span><span class="sxs-lookup"><span data-stu-id="09888-105">This article describes how custom data can be injected by using a VM created with the Azure Service Management API.</span></span> <span data-ttu-id="09888-106">Az Azure erőforrás-kezelési API használatával, olvassa el [a példa sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span><span class="sxs-lookup"><span data-stu-id="09888-106">To see how to use the Azure Resource Management API, see [the example template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span></span>
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a><span data-ttu-id="09888-107">Egyéni adatok beszúrva közzététele az Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="09888-107">Injecting custom data into your Azure virtual machine</span></span>
<span data-ttu-id="09888-108">Ez a funkció jelenleg csak a támogatott a [Azure parancssori felület](https://github.com/Azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="09888-108">This feature is currently supported only in the [Azure Command-Line Interface](https://github.com/Azure/azure-xplat-cli).</span></span> <span data-ttu-id="09888-109">Itt létrehozhatunk egy `custom-data.txt` fájlt, amely tartalmazza az adatokat, majd szúrjon, hogy a virtuális gép kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="09888-109">Here we create a `custom-data.txt` file that contains our data, then inject that in to the VM during provisioning.</span></span> <span data-ttu-id="09888-110">De használhat, a beállításokat a `azure vm create` parancsot a következő egy nagyon egyszerű módszert mutatja be:</span><span class="sxs-lookup"><span data-stu-id="09888-110">Although you may use any of the options for the `azure vm create` command, the following demonstrates one very basic approach:</span></span>

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a><span data-ttu-id="09888-111">Egyéni adatok használata a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="09888-111">Using custom data in the virtual machine</span></span>
* <span data-ttu-id="09888-112">Ha az Azure virtuális gép egy Windows-alapú virtuális Gépet, akkor az egyéni adatok fájlt ment `%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span><span class="sxs-lookup"><span data-stu-id="09888-112">If your Azure VM is a Windows-based VM, then the custom data file is saved to `%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span></span> <span data-ttu-id="09888-113">Bár a helyi számítógépről át az új virtuális gép base64-kódolású volt, akkor automatikusan dekódolt és megnyitható vagy azonnal.</span><span class="sxs-lookup"><span data-stu-id="09888-113">Although it was base64-encoded to transfer from the local computer to the new VM, it is automatically decoded and can be opened or used immediately.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="09888-114">Ha a fájl létezik, a rendszer felülírja.</span><span class="sxs-lookup"><span data-stu-id="09888-114">If the file exists, it is overwritten.</span></span> <span data-ttu-id="09888-115">A könyvtár biztonsági beállítása **System: Full Control** és **rendszergazdák: Full Control**.</span><span class="sxs-lookup"><span data-stu-id="09888-115">The security on the directory is set to **System:Full Control** and **Administrators:Full Control**.</span></span>
  > 
  > 
* <span data-ttu-id="09888-116">Ha az Azure virtuális gép egy Linux-alapú virtuális Gépet, majd egyéni adatok találhatók attól függően, hogy a distro az alábbi helyek egyikét.</span><span class="sxs-lookup"><span data-stu-id="09888-116">If your Azure VM is a Linux-based VM, then the custom data file will be located in one of the following places depending on your distro.</span></span> <span data-ttu-id="09888-117">Valószínűleg az adatok base64 kódolású, szükség lehet az adatok első dekódolási:</span><span class="sxs-lookup"><span data-stu-id="09888-117">The data may be base64-encoded, so you may need to decode the data first:</span></span>
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a><span data-ttu-id="09888-118">Azure cloud inicializálás</span><span class="sxs-lookup"><span data-stu-id="09888-118">Cloud-init on Azure</span></span>
<span data-ttu-id="09888-119">Ha az Azure virtuális gép Ubuntu vagy CoreOS lemezképből CustomData egy felhő-config küldendő felhő inicializálás használhatja.</span><span class="sxs-lookup"><span data-stu-id="09888-119">If your Azure VM is from an Ubuntu or CoreOS image, then you can use CustomData to send a cloud-config to cloud-init.</span></span> <span data-ttu-id="09888-120">Vagy ha az egyéni fájlját egy parancsfájlt, majd felhő inicializálás egyszerűen végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="09888-120">Or if your custom data file is a script, then cloud-init can simply execute it.</span></span>

### <a name="ubuntu-cloud-images"></a><span data-ttu-id="09888-121">Ubuntu felhő lemezképek</span><span class="sxs-lookup"><span data-stu-id="09888-121">Ubuntu Cloud Images</span></span>
<span data-ttu-id="09888-122">A legtöbb Azure Linux-lemezképekben volna szerkesztése "/ etc/waagent.conf" ideiglenes fájl és az ideiglenes erőforrás lemez konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="09888-122">In most Azure Linux images, you would edit "/etc/waagent.conf" to configure the temporary resource disk and swap file.</span></span> <span data-ttu-id="09888-123">Lásd: [Azure Linux ügynök felhasználói útmutató](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt.</span><span class="sxs-lookup"><span data-stu-id="09888-123">See [Azure Linux Agent user guide](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information.</span></span>

<span data-ttu-id="09888-124">Azonban az Ubuntu felhő képek kell használnia felhő inicializálás konfigurálása az erőforrás (Ez azt jelenti, hogy a "elmúló") lemez és partíció felcserélése.</span><span class="sxs-lookup"><span data-stu-id="09888-124">However, on the Ubuntu Cloud Images, you must use cloud-init to configure the resource disk (that is, the "ephemeral" disk) and swap partition.</span></span> <span data-ttu-id="09888-125">A következő oldal jelenik meg a további részletekért Ubuntu wiki: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span><span class="sxs-lookup"><span data-stu-id="09888-125">See the following page on the Ubuntu wiki for more details: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a><span data-ttu-id="09888-126">További lépések: Felhő inicializálás használatával</span><span class="sxs-lookup"><span data-stu-id="09888-126">Next steps: Using cloud-init</span></span>
<span data-ttu-id="09888-127">További információkért lásd: a [Ubuntu felhő inicializálás dokumentációja](https://help.ubuntu.com/community/CloudInit).</span><span class="sxs-lookup"><span data-stu-id="09888-127">For further information, see the [cloud-init documentation for Ubuntu](https://help.ubuntu.com/community/CloudInit).</span></span>

<!--Link references-->
[<span data-ttu-id="09888-128">Adja hozzá a szerepkör Service Management REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="09888-128">Add Role Service Management REST API Reference</span></span>](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[<span data-ttu-id="09888-129">Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="09888-129">Azure Command-line Interface</span></span>](https://github.com/Azure/azure-xplat-cli)

