


<span data-ttu-id="88d17-101">Ez a témakör ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="88d17-101">This topic describes how to:</span></span>

* <span data-ttu-id="88d17-102">Szúrjon be egy Azure virtuális gép (VM) adatokat, amikor folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="88d17-102">Inject data into an Azure virtual machine (VM) when it is being provisioned.</span></span>
* <span data-ttu-id="88d17-103">Beolvasni a Windows és Linux.</span><span class="sxs-lookup"><span data-stu-id="88d17-103">Retrieve it for both Windows and Linux.</span></span>
* <span data-ttu-id="88d17-104">Egyes rendszerek toodetect elérhető speciális eszközöket használja, és automatikusan kezelni az egyéni adatokat.</span><span class="sxs-lookup"><span data-stu-id="88d17-104">Use special tools available on some systems toodetect and handle custom data automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="88d17-105">Ez a cikk ismerteti, hogyan egyéni adatok beszúrhatja egy virtuális Géphez létre hello Azure szolgáltatásfelügyeleti API használatával.</span><span class="sxs-lookup"><span data-stu-id="88d17-105">This article describes how custom data can be injected by using a VM created with hello Azure Service Management API.</span></span> <span data-ttu-id="88d17-106">Hogyan toouse hello Azure erőforrás-kezelési API, lásd: toosee [hello példa sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span><span class="sxs-lookup"><span data-stu-id="88d17-106">toosee how toouse hello Azure Resource Management API, see [hello example template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span></span>
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a><span data-ttu-id="88d17-107">Egyéni adatok beszúrva közzététele az Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="88d17-107">Injecting custom data into your Azure virtual machine</span></span>
<span data-ttu-id="88d17-108">Ez a funkció jelenleg csak a hello támogatott [Azure parancssori felület](https://github.com/Azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="88d17-108">This feature is currently supported only in hello [Azure Command-Line Interface](https://github.com/Azure/azure-xplat-cli).</span></span> <span data-ttu-id="88d17-109">Itt létrehozhatunk egy `custom-data.txt` fájlt, amely tartalmazza az adatokat, majd szúrjon, amely a virtuális gép toohello kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="88d17-109">Here we create a `custom-data.txt` file that contains our data, then inject that in toohello VM during provisioning.</span></span> <span data-ttu-id="88d17-110">Bár használhatja hello beállításokat hello `azure vm create` parancs hello következő egy nagyon egyszerű módszert mutatja be:</span><span class="sxs-lookup"><span data-stu-id="88d17-110">Although you may use any of hello options for hello `azure vm create` command, hello following demonstrates one very basic approach:</span></span>

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-hello-virtual-machine"></a><span data-ttu-id="88d17-111">Egyéni adatok használatának hello virtuális gép</span><span class="sxs-lookup"><span data-stu-id="88d17-111">Using custom data in hello virtual machine</span></span>
* <span data-ttu-id="88d17-112">Ha az Azure virtuális gép egy Windows-alapú virtuális Gépet, akkor egyéni adatfájl hello túl mentett`%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span><span class="sxs-lookup"><span data-stu-id="88d17-112">If your Azure VM is a Windows-based VM, then hello custom data file is saved too`%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span></span> <span data-ttu-id="88d17-113">Bár a helyi számítógép toohello hello base64-kódolású tootransfer volt új virtuális Gépet, akkor a rendszer automatikusan dekódolni, és megnyitható vagy azonnal.</span><span class="sxs-lookup"><span data-stu-id="88d17-113">Although it was base64-encoded tootransfer from hello local computer toohello new VM, it is automatically decoded and can be opened or used immediately.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="88d17-114">Ha hello fájl létezik, a rendszer felülírja.</span><span class="sxs-lookup"><span data-stu-id="88d17-114">If hello file exists, it is overwritten.</span></span> <span data-ttu-id="88d17-115">hello biztonsági hello címtár értéke túl**System: Full Control** és **rendszergazdák: Full Control**.</span><span class="sxs-lookup"><span data-stu-id="88d17-115">hello security on hello directory is set too**System:Full Control** and **Administrators:Full Control**.</span></span>
  > 
  > 
* <span data-ttu-id="88d17-116">Esetén az Azure virtuális gép a Linux-alapú virtuális gépek hello egyéni adatok fájl is található, a következő hello attól függően, hogy a distro helyezi.</span><span class="sxs-lookup"><span data-stu-id="88d17-116">If your Azure VM is a Linux-based VM, then hello custom data file will be located in one of hello following places depending on your distro.</span></span> <span data-ttu-id="88d17-117">hello történhet base64-kódolású, ezért először esetleg toodecode hello adatok:</span><span class="sxs-lookup"><span data-stu-id="88d17-117">hello data may be base64-encoded, so you may need toodecode hello data first:</span></span>
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a><span data-ttu-id="88d17-118">Azure cloud inicializálás</span><span class="sxs-lookup"><span data-stu-id="88d17-118">Cloud-init on Azure</span></span>
<span data-ttu-id="88d17-119">Ha az Azure virtuális gép Ubuntu vagy CoreOS lemezképből, majd használhatja CustomData toosend egy felhő-config toocloud inicializálás.</span><span class="sxs-lookup"><span data-stu-id="88d17-119">If your Azure VM is from an Ubuntu or CoreOS image, then you can use CustomData toosend a cloud-config toocloud-init.</span></span> <span data-ttu-id="88d17-120">Vagy ha az egyéni fájlját egy parancsfájlt, majd felhő inicializálás egyszerűen végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="88d17-120">Or if your custom data file is a script, then cloud-init can simply execute it.</span></span>

### <a name="ubuntu-cloud-images"></a><span data-ttu-id="88d17-121">Ubuntu felhő lemezképek</span><span class="sxs-lookup"><span data-stu-id="88d17-121">Ubuntu Cloud Images</span></span>
<span data-ttu-id="88d17-122">A legtöbb Azure Linux-lemezképekben volna szerkesztése "/ etc/waagent.conf" tooconfigure hello ideiglenes lemezes és a lapozófájl-kapacitás erőforrásfájl.</span><span class="sxs-lookup"><span data-stu-id="88d17-122">In most Azure Linux images, you would edit "/etc/waagent.conf" tooconfigure hello temporary resource disk and swap file.</span></span> <span data-ttu-id="88d17-123">Lásd: [Azure Linux ügynök felhasználói útmutató](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt.</span><span class="sxs-lookup"><span data-stu-id="88d17-123">See [Azure Linux Agent user guide](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information.</span></span>

<span data-ttu-id="88d17-124">Azonban a hello Ubuntu felhő képek, és kell használnia felhő inicializálás tooconfigure hello erőforrás (Ez azt jelenti, hogy hello "elmúló") lemez swap partíció.</span><span class="sxs-lookup"><span data-stu-id="88d17-124">However, on hello Ubuntu Cloud Images, you must use cloud-init tooconfigure hello resource disk (that is, hello "ephemeral" disk) and swap partition.</span></span> <span data-ttu-id="88d17-125">Hello lap következő hello Ubuntu wiki további részletekért lásd: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span><span class="sxs-lookup"><span data-stu-id="88d17-125">See hello following page on hello Ubuntu wiki for more details: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps-using-cloud-init"></a><span data-ttu-id="88d17-126">További lépések: Felhő inicializálás használatával</span><span class="sxs-lookup"><span data-stu-id="88d17-126">Next steps: Using cloud-init</span></span>
<span data-ttu-id="88d17-127">További információkért lásd: hello [Ubuntu felhő inicializálás dokumentációja](https://help.ubuntu.com/community/CloudInit).</span><span class="sxs-lookup"><span data-stu-id="88d17-127">For further information, see hello [cloud-init documentation for Ubuntu](https://help.ubuntu.com/community/CloudInit).</span></span>

<!--Link references-->
[<span data-ttu-id="88d17-128">Adja hozzá a szerepkör Service Management REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="88d17-128">Add Role Service Management REST API Reference</span></span>](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[<span data-ttu-id="88d17-129">Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="88d17-129">Azure Command-line Interface</span></span>](https://github.com/Azure/azure-xplat-cli)

