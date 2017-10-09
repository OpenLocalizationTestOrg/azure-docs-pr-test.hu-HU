## <a name="overview"></a><span data-ttu-id="ec212-101">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ec212-101">Overview</span></span>
<span data-ttu-id="ec212-102">Amikor hoz létre egy új virtuális gép (VM) erőforráscsoportban lemezképek [Azure piactér](https://azure.microsoft.com/marketplace/), hello alapértelmezett operációsrendszer-meghajtón 127 GB.</span><span class="sxs-lookup"><span data-stu-id="ec212-102">When you create a new virtual machine (VM) in a Resource Group by deploying an image from [Azure Marketplace](https://azure.microsoft.com/marketplace/), hello default OS drive is 127 GB.</span></span> <span data-ttu-id="ec212-103">Annak ellenére, hogy lehetséges tooadd adatok lemezek toohello (hello SKU számától függően a választott) virtuális gép, és továbbá ajánlott tooinstall alkalmazások és a CPU-igényes munkaterhelések ezeket a kiegészítő lemezeken, gyakran kell tooexpand hello az operációs rendszer meghajtó toosupport bizonyos forgatókönyvekben, például a következőket:</span><span class="sxs-lookup"><span data-stu-id="ec212-103">Even though it’s possible tooadd data disks toohello VM (how many depending upon hello SKU you’ve chosen) and moreover it’s recommended tooinstall applications and CPU intensive workloads on these addendum disks, oftentimes customers need tooexpand hello OS drive toosupport certain scenarios such as following:</span></span>

1. <span data-ttu-id="ec212-104">Az operációs rendszer meghajtójára összetevőket telepítő, régebbi alkalmazások támogatása.</span><span class="sxs-lookup"><span data-stu-id="ec212-104">Support legacy applications that install components on OS drive.</span></span>
2. <span data-ttu-id="ec212-105">Nagyobb operációsrendszer-meghajtóval rendelkező fizikai számítógép vagy virtuális gép migrálása a helyszínről.</span><span class="sxs-lookup"><span data-stu-id="ec212-105">Migrate a physical PC or virtual machine from on-premises with a larger OS drive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec212-106">Az Azure két különböző üzemi modellel rendelkezik az erőforrások létrehozásához és használatához: Resource Manager-alapú és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="ec212-106">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="ec212-107">Ez a cikk ismerteti a hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="ec212-107">This article covers using hello Resource Manager model.</span></span> <span data-ttu-id="ec212-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="ec212-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> 
> 

## <a name="resize-hello-os-drive"></a><span data-ttu-id="ec212-109">Az operációs rendszer hello meghajtó átméretezése</span><span class="sxs-lookup"><span data-stu-id="ec212-109">Resize hello OS drive</span></span>
<span data-ttu-id="ec212-110">Ebben a cikkben azt fogja a feladatnak hello a resource manager modulja segítségével az operációs rendszer hello meghajtó átméretezése [Azure Powershell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="ec212-110">In this article we’ll accomplish hello task of resizing hello OS drive using resource manager modules of [Azure Powershell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="ec212-111">Felügyeleti üzemmódban megnyitott a Powershell ISE vagy a Powershell-ablakot, és kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ec212-111">Open your Powershell ISE or Powershell window in administrative mode and follow hello steps below:</span></span>

1. <span data-ttu-id="ec212-112">Bejelentkezési tooyour Microsoft Azure erőforrás-kezelő módban a fiókot, és jelölje ki az előfizetését a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="ec212-112">Sign-in tooyour Microsoft Azure account in resource management mode and select your subscription as follows:</span></span>
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. <span data-ttu-id="ec212-113">Állítsa az erőforráscsoport és a virtuális gép nevét a következőre:</span><span class="sxs-lookup"><span data-stu-id="ec212-113">Set your resource group name and VM name as follows:</span></span>
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. <span data-ttu-id="ec212-114">Szerezzen be egy hivatkozási tooyour VM az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ec212-114">Obtain a reference tooyour VM as follows:</span></span>
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. <span data-ttu-id="ec212-115">Hello VM leállítása előtt hello lemez átméretezése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ec212-115">Stop hello VM before resizing hello disk as follows:</span></span>
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. <span data-ttu-id="ec212-116">Tesztelhet, és itt azt vár a hello jelenleg!</span><span class="sxs-lookup"><span data-stu-id="ec212-116">And here comes hello moment we’ve been waiting for!</span></span> <span data-ttu-id="ec212-117">Állítsa be a hello az operációs rendszer szükséges toohello érték hello méretét, és hello VM frissítése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ec212-117">Set hello size of hello OS disk toohello desired value and update hello VM as follows:</span></span>
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > <span data-ttu-id="ec212-118">hello új méret hello meglévő lemez méretének nagyobbnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ec212-118">hello new size should be greater than hello existing disk size.</span></span> <span data-ttu-id="ec212-119">hello maximális engedélyezett 1023 GB-os.</span><span class="sxs-lookup"><span data-stu-id="ec212-119">hello maximum allowed is 1023 GB.</span></span>
   > 
   > 
6. <span data-ttu-id="ec212-120">Virtuális gép frissítése hello néhány másodpercet vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="ec212-120">Updating hello VM may take a few seconds.</span></span> <span data-ttu-id="ec212-121">Hello parancs futtatásának befejeztével végrehajtása után indítsa újra a virtuális gép hello az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ec212-121">Once hello command finishes executing, restart hello VM as follows:</span></span>
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

<span data-ttu-id="ec212-122">Készen is van.</span><span class="sxs-lookup"><span data-stu-id="ec212-122">And that’s it!</span></span> <span data-ttu-id="ec212-123">Most a virtuális gép, hello RDP nyissa meg a számítógép-kezelés (vagy a Lemezkezelés eszköz), és bontsa ki az újonnan lefoglalt terület hello segítségével hello meghajtó.</span><span class="sxs-lookup"><span data-stu-id="ec212-123">Now RDP into hello VM, open Computer Management (or Disk Management) and expand hello drive using hello newly allocated space.</span></span>

## <a name="summary"></a><span data-ttu-id="ec212-124">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ec212-124">Summary</span></span>
<span data-ttu-id="ec212-125">Ebben a cikkben Powershell tooexpand hello az operációs rendszer meghajtóját a IaaS virtuális gépként az Azure Resource Manager modulok használtuk.</span><span class="sxs-lookup"><span data-stu-id="ec212-125">In this article, we used Azure Resource Manager modules of Powershell tooexpand hello OS drive of an IaaS virtual machine.</span></span> <span data-ttu-id="ec212-126">Hello referenciaként a teljes parancsfájl másolható alatt van:</span><span class="sxs-lookup"><span data-stu-id="ec212-126">Reproduced below is hello complete script for your reference:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ec212-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec212-127">Next Steps</span></span>
<span data-ttu-id="ec212-128">Bár ebben a cikkben azt összpontosított elsősorban a virtuális gép hello hello OS lemez bővítése, hello fejlett parancsfájl is felhasználhatók a bővített hello adatok lemezek csatolt toohello VM egyetlen kódsort módosításával.</span><span class="sxs-lookup"><span data-stu-id="ec212-128">Though in this article, we focused primarily on expanding hello OS disk of hello VM, hello developed script may also be used for expanding hello data disks attached toohello VM by changing a single line of code.</span></span> <span data-ttu-id="ec212-129">Például tooexpand hello első adatok lemezre csatlakoztatott toohello VM, cserélje le a hello ```OSDisk``` objektum ```StorageProfile``` rendelkező ```DataDisks``` tömb, és egy numerikus index tooobtain hivatkozás toofirst csatolt adatlemezt, használja az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="ec212-129">For example, tooexpand hello first data disk attached toohello VM, replace hello ```OSDisk``` object of ```StorageProfile``` with ```DataDisks``` array and use a numeric index tooobtain a reference toofirst attached data disk, as shown below:</span></span>

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
<span data-ttu-id="ec212-130">Hasonló módon lehet, hogy más adatok lemezek csatolt toohello index használatával, ahogy fent látható, a virtuális gép hivatkozik, vagy hello ```Name``` tulajdonság hello lemez, az alábbi ábra szemlélteti:</span><span class="sxs-lookup"><span data-stu-id="ec212-130">Similarly you may reference other data disks attached toohello VM, either by using an index as shown above or hello ```Name``` property of hello disk as illustrated below:</span></span>

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

<span data-ttu-id="ec212-131">Ha azt szeretné, hogy ki hogyan tooattach lemezek tooan Azure Resource Manager virtuális gép toofind, ellenőrizze a [cikk](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ec212-131">If you want toofind out how tooattach disks tooan Azure Resource Manager VM, check this [article](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

