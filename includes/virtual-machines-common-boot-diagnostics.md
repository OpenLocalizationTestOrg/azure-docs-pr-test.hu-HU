<span data-ttu-id="35c44-101">Mostantól két hibakereső szolgáltatás is elérhető az Azure-ban: konzolkimenet és képernyőkép is támogatott az Azure virtuális gépeken a Resource Manager-alapú üzemi modellben.</span><span class="sxs-lookup"><span data-stu-id="35c44-101">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="35c44-102">Amikor a saját rendszerképét használja az Azure-ban, vagy valamelyik platform rendszerképét indítja, számos oka lehet annak, hogy egy virtuális gép rendszerindításra képtelen állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="35c44-102">When bringing your own image to Azure or even booting one of the platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="35c44-103">Ezekkel a szolgáltatásokkal könnyedén diagnosztizálhatja és helyreállíthatja a virtuális gépeket a rendszerindítási hibák után.</span><span class="sxs-lookup"><span data-stu-id="35c44-103">These features enable you to easily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="35c44-104">Linux virtuális gépek esetén a konzol naplófájljának kimenetét egyszerűen megtekintheti a Portalon:</span><span class="sxs-lookup"><span data-stu-id="35c44-104">For Linux Virtual Machines, you can easily view the output of your console log from the Portal:</span></span>

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="35c44-106">Azonban az Azure Windows és Linux virtuális gépeken is lehetővé teszi, hogy megtekintsen egy képernyőképet a virtuális gépről a hipervizortól:</span><span class="sxs-lookup"><span data-stu-id="35c44-106">However, for both Windows and Linux Virtual Machines, Azure also enables you to see a screenshot of the VM from the hypervisor:</span></span>

![Hiba](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

<span data-ttu-id="35c44-108">Mindkét szolgáltatás támogatott az Azure virtuális gépeken minden régióban.</span><span class="sxs-lookup"><span data-stu-id="35c44-108">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="35c44-109">Ne feledje, akár 10 percet is igénybe vehet, hogy a képernyőképek és a kimenet megjelenjen a tárfiókjában.</span><span class="sxs-lookup"><span data-stu-id="35c44-109">Note, screenshots, and output can take up to 10 minutes to appear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="35c44-110">Gyakori rendszerindítási hibák</span><span class="sxs-lookup"><span data-stu-id="35c44-110">Common boot errors</span></span>

- [<span data-ttu-id="35c44-111">0xC000000E</span><span class="sxs-lookup"><span data-stu-id="35c44-111">0xC000000E</span></span>](https://support.microsoft.com/help/4010129)
- [<span data-ttu-id="35c44-112">0xC000000F</span><span class="sxs-lookup"><span data-stu-id="35c44-112">0xC000000F</span></span>](https://support.microsoft.com/help/4010130)
- [<span data-ttu-id="35c44-113">0xC0000011</span><span class="sxs-lookup"><span data-stu-id="35c44-113">0xC0000011</span></span>](https://support.microsoft.com/help/4010134)
- [<span data-ttu-id="35c44-114">0xC0000034</span><span class="sxs-lookup"><span data-stu-id="35c44-114">0xC0000034</span></span>](https://support.microsoft.com/help/4010140)
- [<span data-ttu-id="35c44-115">0xC0000098</span><span class="sxs-lookup"><span data-stu-id="35c44-115">0xC0000098</span></span>](https://support.microsoft.com/help/4010137)
- [<span data-ttu-id="35c44-116">0xC00000BA</span><span class="sxs-lookup"><span data-stu-id="35c44-116">0xC00000BA</span></span>](https://support.microsoft.com/help/4010136)
- [<span data-ttu-id="35c44-117">0xC000014C</span><span class="sxs-lookup"><span data-stu-id="35c44-117">0xC000014C</span></span>](https://support.microsoft.com/help/4010141)
- [<span data-ttu-id="35c44-118">0xC0000221</span><span class="sxs-lookup"><span data-stu-id="35c44-118">0xC0000221</span></span>](https://support.microsoft.com/help/4010132)
- [<span data-ttu-id="35c44-119">0xC0000225</span><span class="sxs-lookup"><span data-stu-id="35c44-119">0xC0000225</span></span>](https://support.microsoft.com/help/4010138)
- [<span data-ttu-id="35c44-120">0xC0000359</span><span class="sxs-lookup"><span data-stu-id="35c44-120">0xC0000359</span></span>](https://support.microsoft.com/help/4010135)
- [<span data-ttu-id="35c44-121">0xC0000605</span><span class="sxs-lookup"><span data-stu-id="35c44-121">0xC0000605</span></span>](https://support.microsoft.com/help/4010131)
- [<span data-ttu-id="35c44-122">Nem található operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="35c44-122">An operating system wasn't found</span></span>](https://support.microsoft.com/help/4010142)
- [<span data-ttu-id="35c44-123">Rendszerindítási hiba vagy INACCESSIBLE_BOOT_DEVICE</span><span class="sxs-lookup"><span data-stu-id="35c44-123">Boot failure or INACCESSIBLE_BOOT_DEVICE</span></span>](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="35c44-124">Diagnosztika engedélyezése egy új virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="35c44-124">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="35c44-125">Amikor új virtuális gépet hoz létre a Betekintő portálon, válassza az **Azure Resource Manager** lehetőséget az üzemi modell legördülő menüből:</span><span class="sxs-lookup"><span data-stu-id="35c44-125">When creating a new Virtual Machine from the Preview Portal, select the **Azure Resource Manager** from the deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="35c44-127">Konfigurálja a Figyelés beállítást, és válassza ki a tárfiókot, ahova el kívánja helyezni ezeket a diagnosztikai fájlokat.</span><span class="sxs-lookup"><span data-stu-id="35c44-127">Configure the Monitoring option to select the storage account where you would like to place these diagnostic files.</span></span>
 
    ![Virtuális gép létrehozása](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="35c44-129">Ha egy Azure Resource Manager-sablonból végzi a központi telepítést, keresse meg a virtuális gép erőforrást, és fűzze hozzá a diagnosztikai profil szakaszt.</span><span class="sxs-lookup"><span data-stu-id="35c44-129">If you are deploying from an Azure Resource Manager template, navigate to your Virtual Machine resource and append the diagnostics profile section.</span></span> <span data-ttu-id="35c44-130">Fontos, hogy a „2015-06-15” API-verzió fejlécet használja.</span><span class="sxs-lookup"><span data-stu-id="35c44-130">Remember to use the “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="35c44-131">A diagnosztikai profil lehetővé teszi, hogy kiválassza a tárfiókot, ahol el kívánja helyezni ezeket a naplókat.</span><span class="sxs-lookup"><span data-stu-id="35c44-131">The diagnostics profile enables you to select the storage account where you want to put these logs.</span></span>

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

<span data-ttu-id="35c44-132">Engedélyezett rendszerindítási diagnosztikával futó minta virtuális gép telepítéséhez tekintse meg a tárházunkat.</span><span class="sxs-lookup"><span data-stu-id="35c44-132">To deploy a sample Virtual Machine with boot diagnostics enabled, check out our repo here.</span></span>

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="35c44-133">Meglévő virtuális gép frissítése</span><span class="sxs-lookup"><span data-stu-id="35c44-133">Update an existing virtual machine</span></span> ##

<span data-ttu-id="35c44-134">Ha a portálon keresztül kívánja engedélyezni a rendszerindítási diagnosztikát, frissíthet egy meglévő virtuális gépet is.</span><span class="sxs-lookup"><span data-stu-id="35c44-134">To enable boot diagnostics through the Portal, you can also update an existing Virtual Machine through the Portal.</span></span> <span data-ttu-id="35c44-135">Válassza a Rendszerindítási diagnosztika lehetőséget, majd kattintson a Mentés elemre.</span><span class="sxs-lookup"><span data-stu-id="35c44-135">Select the Boot Diagnostics option and Save.</span></span> <span data-ttu-id="35c44-136">A módosítás a virtuális gép újraindítása után lép életbe.</span><span class="sxs-lookup"><span data-stu-id="35c44-136">Restart the VM to take effect.</span></span>

![Létező virtuális gép frissítése](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

