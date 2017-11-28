## <a name="overview"></a><span data-ttu-id="47cf0-101">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="47cf0-101">Overview</span></span>
<span data-ttu-id="47cf0-102">Amikor létrehoz egy új virtuális gépet egy erőforráscsoportban egy [Azure Marketplace-ről](https://azure.microsoft.com/marketplace/) származó rendszerképből, az operációs rendszer meghajtójának alapértelmezett mérete 127 GB.</span><span class="sxs-lookup"><span data-stu-id="47cf0-102">When you create a new virtual machine (VM) in a Resource Group by deploying an image from [Azure Marketplace](https://azure.microsoft.com/marketplace/), the default OS drive is 127 GB.</span></span> <span data-ttu-id="47cf0-103">Bár hozzáadhat adatlemezeket a virtuális géphez (ezeknek száma a választott termékváltozattól függ), és ajánlott ezekre a kiegészítő lemezekre telepíteni az alkalmazásokat és a processzorigényes számítási feladatokat, az ügyfeleknek gyakran bővíteniük kell az operációs rendszer meghajtóját bizonyos forgatókönyvek támogatásához, mint például a következők:</span><span class="sxs-lookup"><span data-stu-id="47cf0-103">Even though it’s possible to add data disks to the VM (how many depending upon the SKU you’ve chosen) and moreover it’s recommended to install applications and CPU intensive workloads on these addendum disks, oftentimes customers need to expand the OS drive to support certain scenarios such as following:</span></span>

1. <span data-ttu-id="47cf0-104">Az operációs rendszer meghajtójára összetevőket telepítő, régebbi alkalmazások támogatása.</span><span class="sxs-lookup"><span data-stu-id="47cf0-104">Support legacy applications that install components on OS drive.</span></span>
2. <span data-ttu-id="47cf0-105">Nagyobb operációsrendszer-meghajtóval rendelkező fizikai számítógép vagy virtuális gép migrálása a helyszínről.</span><span class="sxs-lookup"><span data-stu-id="47cf0-105">Migrate a physical PC or virtual machine from on-premises with a larger OS drive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="47cf0-106">Az Azure két különböző üzemi modellel rendelkezik az erőforrások létrehozásához és használatához: Resource Manager-alapú és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="47cf0-106">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="47cf0-107">Ez a cikk a Resource Manager-alapú modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="47cf0-107">This article covers using the Resource Manager model.</span></span> <span data-ttu-id="47cf0-108">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="47cf0-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> 
> 

## <a name="resize-the-os-drive"></a><span data-ttu-id="47cf0-109">Az operációs rendszer meghajtójának átméretezése</span><span class="sxs-lookup"><span data-stu-id="47cf0-109">Resize the OS drive</span></span>
<span data-ttu-id="47cf0-110">Ez a cikk az operációs rendszer meghajtójának az [Azure Powershell](/powershell/azureps-cmdlets-docs) erőforrás-kezelő moduljaival történő átméretezését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="47cf0-110">In this article we’ll accomplish the task of resizing the OS drive using resource manager modules of [Azure Powershell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="47cf0-111">Nyissa meg a Powershell ISE-t vagy PowerShell-ablakot rendszergazdai módban, és kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="47cf0-111">Open your Powershell ISE or Powershell window in administrative mode and follow the steps below:</span></span>

1. <span data-ttu-id="47cf0-112">Jelentkezzen be a Microsoft Azure-fiókjába erőforrás-kezelő módban, és válassza ki az előfizetését a következő módon:</span><span class="sxs-lookup"><span data-stu-id="47cf0-112">Sign-in to your Microsoft Azure account in resource management mode and select your subscription as follows:</span></span>
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. <span data-ttu-id="47cf0-113">Állítsa az erőforráscsoport és a virtuális gép nevét a következőre:</span><span class="sxs-lookup"><span data-stu-id="47cf0-113">Set your resource group name and VM name as follows:</span></span>
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. <span data-ttu-id="47cf0-114">Szerezzen be egy hivatkozást a virtuális gépre a következő módon:</span><span class="sxs-lookup"><span data-stu-id="47cf0-114">Obtain a reference to your VM as follows:</span></span>
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. <span data-ttu-id="47cf0-115">A lemez átméretezése előtt állítsa le a virtuális gépet a következő módon:</span><span class="sxs-lookup"><span data-stu-id="47cf0-115">Stop the VM before resizing the disk as follows:</span></span>
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. <span data-ttu-id="47cf0-116">Íme a pillanat, amire vártunk!</span><span class="sxs-lookup"><span data-stu-id="47cf0-116">And here comes the moment we’ve been waiting for!</span></span> <span data-ttu-id="47cf0-117">Állítsa az operációsrendszer-lemez méretét a kívánt értékre, és frissítse a virtuális gépet a következő módon:</span><span class="sxs-lookup"><span data-stu-id="47cf0-117">Set the size of the OS disk to the desired value and update the VM as follows:</span></span>
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > <span data-ttu-id="47cf0-118">Az új méretnek nagyobbnak kell lennie a meglévő lemezméretnél.</span><span class="sxs-lookup"><span data-stu-id="47cf0-118">The new size should be greater than the existing disk size.</span></span> <span data-ttu-id="47cf0-119">A maximálisan megengedett méret 1023 GB.</span><span class="sxs-lookup"><span data-stu-id="47cf0-119">The maximum allowed is 1023 GB.</span></span>
   > 
   > 
6. <span data-ttu-id="47cf0-120">A virtuális gép frissítése eltarthat néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="47cf0-120">Updating the VM may take a few seconds.</span></span> <span data-ttu-id="47cf0-121">Miután a parancs végrehajtása befejeződött, indítsa újra a virtuális gépet a következő módon:</span><span class="sxs-lookup"><span data-stu-id="47cf0-121">Once the command finishes executing, restart the VM as follows:</span></span>
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

<span data-ttu-id="47cf0-122">Készen is van.</span><span class="sxs-lookup"><span data-stu-id="47cf0-122">And that’s it!</span></span> <span data-ttu-id="47cf0-123">Csatlakozzon RDP-kapcsolaton keresztül a virtuális géphez, nyissa meg a Számítógép-kezelés (vagy Lemezkezelés) elemet, és bővítse ki a meghajtót az újonnan kiosztott tárhellyel.</span><span class="sxs-lookup"><span data-stu-id="47cf0-123">Now RDP into the VM, open Computer Management (or Disk Management) and expand the drive using the newly allocated space.</span></span>

## <a name="summary"></a><span data-ttu-id="47cf0-124">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="47cf0-124">Summary</span></span>
<span data-ttu-id="47cf0-125">Ebben a cikkben a Powershell Azure Resource Manager-moduljaival bővítettük egy IaaS-beli virtuális gép operációsrendszer-meghajtóját.</span><span class="sxs-lookup"><span data-stu-id="47cf0-125">In this article, we used Azure Resource Manager modules of Powershell to expand the OS drive of an IaaS virtual machine.</span></span> <span data-ttu-id="47cf0-126">Az alábbiakban találja a teljes szkriptet:</span><span class="sxs-lookup"><span data-stu-id="47cf0-126">Reproduced below is the complete script for your reference:</span></span>

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a><span data-ttu-id="47cf0-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47cf0-127">Next Steps</span></span>
<span data-ttu-id="47cf0-128">Ez a cikk elsősorban a virtuális gép operációsrendszer-lemezének bővítéséről szól, azonban a létrehozott szkript egyetlen sor módosításával a virtuális géphez csatolt adatlemezek bővítésére is használható.</span><span class="sxs-lookup"><span data-stu-id="47cf0-128">Though in this article, we focused primarily on expanding the OS disk of the VM, the developed script may also be used for expanding the data disks attached to the VM by changing a single line of code.</span></span> <span data-ttu-id="47cf0-129">A virtuális géphez csatolt első adatlemez bővítéséhez például cserélje ki a ```StorageProfile``` ```OSDisk``` objektumát a ```DataDisks``` tömbre, és egy numerikus indexszel szerezzen be az első csatolt adatlemezre mutató hivatkozást az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="47cf0-129">For example, to expand the first data disk attached to the VM, replace the ```OSDisk``` object of ```StorageProfile``` with ```DataDisks``` array and use a numeric index to obtain a reference to first attached data disk, as shown below:</span></span>

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
<span data-ttu-id="47cf0-130">Hasonlóképpen hivatkozhat más, a virtuális géphez csatolt adatlemezekre is, akár a fent látható módon egy index használatával, akár a lemez ```Name``` tulajdonságával, az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="47cf0-130">Similarly you may reference other data disks attached to the VM, either by using an index as shown above or the ```Name``` property of the disk as illustrated below:</span></span>

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

<span data-ttu-id="47cf0-131">Ha arról szeretne tájékozódni, hogyan csatlakoztathat lemezeket egy Azure Resource Manager-alapú virtuális géphez, tekintse meg ezt a [cikket](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="47cf0-131">If you want to find out how to attach disks to an Azure Resource Manager VM, check this [article](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

