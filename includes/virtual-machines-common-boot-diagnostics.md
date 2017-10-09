<span data-ttu-id="e02a4-101">Mostantól két hibakereső szolgáltatás is elérhető az Azure-ban: konzolkimenet és képernyőkép is támogatott az Azure virtuális gépeken a Resource Manager-alapú üzemi modellben.</span><span class="sxs-lookup"><span data-stu-id="e02a4-101">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="e02a4-102">Amikor a saját kép tooAzure vagy hello platform képek is rendszerindítás egyike, miért egy virtuális gép nem indítható állapotba lekérdezi számos oka lehet.</span><span class="sxs-lookup"><span data-stu-id="e02a4-102">When bringing your own image tooAzure or even booting one of hello platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="e02a4-103">E szolgáltatások engedélyezése meg tooeasily diagnosztizálhatja és a virtuális gépek helyreállítás rendszerindítási hibák esetén.</span><span class="sxs-lookup"><span data-stu-id="e02a4-103">These features enable you tooeasily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="e02a4-104">A Linux virtuális gépek megtekintéséhez a konzol napló, a portál hello hello kimeneti:</span><span class="sxs-lookup"><span data-stu-id="e02a4-104">For Linux Virtual Machines, you can easily view hello output of your console log from hello Portal:</span></span>

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="e02a4-106">Azonban a Windows és Linux rendszerű virtuális gépek Azure is lehetővé teszi, hogy egy képernyőfelvétel a hello VM hello hipervizorról toosee:</span><span class="sxs-lookup"><span data-stu-id="e02a4-106">However, for both Windows and Linux Virtual Machines, Azure also enables you toosee a screenshot of hello VM from hello hypervisor:</span></span>

![Hiba](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

<span data-ttu-id="e02a4-108">Mindkét szolgáltatás támogatott az Azure virtuális gépeken minden régióban.</span><span class="sxs-lookup"><span data-stu-id="e02a4-108">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="e02a4-109">Vegye figyelembe, a pillanatképek és a kimeneti too10 perc tooappear a tárfiókban lévő is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="e02a4-109">Note, screenshots, and output can take up too10 minutes tooappear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="e02a4-110">Gyakori rendszerindítási hibák</span><span class="sxs-lookup"><span data-stu-id="e02a4-110">Common boot errors</span></span>

- [<span data-ttu-id="e02a4-111">0xC000000E</span><span class="sxs-lookup"><span data-stu-id="e02a4-111">0xC000000E</span></span>](https://support.microsoft.com/help/4010129)
- [<span data-ttu-id="e02a4-112">0xC000000F</span><span class="sxs-lookup"><span data-stu-id="e02a4-112">0xC000000F</span></span>](https://support.microsoft.com/help/4010130)
- [<span data-ttu-id="e02a4-113">0xC0000011</span><span class="sxs-lookup"><span data-stu-id="e02a4-113">0xC0000011</span></span>](https://support.microsoft.com/help/4010134)
- [<span data-ttu-id="e02a4-114">0xC0000034</span><span class="sxs-lookup"><span data-stu-id="e02a4-114">0xC0000034</span></span>](https://support.microsoft.com/help/4010140)
- [<span data-ttu-id="e02a4-115">0xC0000098</span><span class="sxs-lookup"><span data-stu-id="e02a4-115">0xC0000098</span></span>](https://support.microsoft.com/help/4010137)
- [<span data-ttu-id="e02a4-116">0xC00000BA</span><span class="sxs-lookup"><span data-stu-id="e02a4-116">0xC00000BA</span></span>](https://support.microsoft.com/help/4010136)
- [<span data-ttu-id="e02a4-117">0xC000014C</span><span class="sxs-lookup"><span data-stu-id="e02a4-117">0xC000014C</span></span>](https://support.microsoft.com/help/4010141)
- [<span data-ttu-id="e02a4-118">0xC0000221</span><span class="sxs-lookup"><span data-stu-id="e02a4-118">0xC0000221</span></span>](https://support.microsoft.com/help/4010132)
- [<span data-ttu-id="e02a4-119">0xC0000225</span><span class="sxs-lookup"><span data-stu-id="e02a4-119">0xC0000225</span></span>](https://support.microsoft.com/help/4010138)
- [<span data-ttu-id="e02a4-120">0xC0000359</span><span class="sxs-lookup"><span data-stu-id="e02a4-120">0xC0000359</span></span>](https://support.microsoft.com/help/4010135)
- [<span data-ttu-id="e02a4-121">0xC0000605</span><span class="sxs-lookup"><span data-stu-id="e02a4-121">0xC0000605</span></span>](https://support.microsoft.com/help/4010131)
- [<span data-ttu-id="e02a4-122">Nem található operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="e02a4-122">An operating system wasn't found</span></span>](https://support.microsoft.com/help/4010142)
- [<span data-ttu-id="e02a4-123">Rendszerindítási hiba vagy INACCESSIBLE_BOOT_DEVICE</span><span class="sxs-lookup"><span data-stu-id="e02a4-123">Boot failure or INACCESSIBLE_BOOT_DEVICE</span></span>](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="e02a4-124">Diagnosztika engedélyezése egy új virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="e02a4-124">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="e02a4-125">Ha egy új virtuális gép létrehozása a betekintő portálon hello, válassza ki a hello **Azure Resource Manager** hello telepítési modell legördülő listából:</span><span class="sxs-lookup"><span data-stu-id="e02a4-125">When creating a new Virtual Machine from hello Preview Portal, select hello **Azure Resource Manager** from hello deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="e02a4-127">A diagnosztikai fájlok hello figyelés beállítás tooselect hello tárfiók tooplace helyének megadása</span><span class="sxs-lookup"><span data-stu-id="e02a4-127">Configure hello Monitoring option tooselect hello storage account where you would like tooplace these diagnostic files.</span></span>
 
    ![Virtuális gép létrehozása](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="e02a4-129">Ha telepíti az Azure Resource Manager sablon alapján, keresse meg a virtuálisgép-erőforrás tooyour, és a hozzáfűző hello diagnosztikai profil szakasz.</span><span class="sxs-lookup"><span data-stu-id="e02a4-129">If you are deploying from an Azure Resource Manager template, navigate tooyour Virtual Machine resource and append hello diagnostics profile section.</span></span> <span data-ttu-id="e02a4-130">Ne felejtse el toouse hello "2015-06-15" API version fejlécnek.</span><span class="sxs-lookup"><span data-stu-id="e02a4-130">Remember toouse hello “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="e02a4-131">hello diagnosztikai profiljának lehetővé teszi tooselect hello tárfiók, amelybe tooput ezek a naplók.</span><span class="sxs-lookup"><span data-stu-id="e02a4-131">hello diagnostics profile enables you tooselect hello storage account where you want tooput these logs.</span></span>

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

<span data-ttu-id="e02a4-132">a rendszerindítási diagnosztika engedélyezve van, itt a tárházban kivételét minta virtuális gép toodeploy.</span><span class="sxs-lookup"><span data-stu-id="e02a4-132">toodeploy a sample Virtual Machine with boot diagnostics enabled, check out our repo here.</span></span>

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="e02a4-133">Meglévő virtuális gép frissítése</span><span class="sxs-lookup"><span data-stu-id="e02a4-133">Update an existing virtual machine</span></span> ##

<span data-ttu-id="e02a4-134">hello portálon keresztül tooenable a rendszerindítási diagnosztika, is frissítheti egy meglévő virtuális gép hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="e02a4-134">tooenable boot diagnostics through hello Portal, you can also update an existing Virtual Machine through hello Portal.</span></span> <span data-ttu-id="e02a4-135">Válassza ki a rendszerindítási diagnosztika lehetőséget, majd mentse hello.</span><span class="sxs-lookup"><span data-stu-id="e02a4-135">Select hello Boot Diagnostics option and Save.</span></span> <span data-ttu-id="e02a4-136">Hello VM tootake léptetéséhez indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="e02a4-136">Restart hello VM tootake effect.</span></span>

![Létező virtuális gép frissítése](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

