
<span data-ttu-id="4f916-101">További információ a lemezekkel kapcsolatban: [A lemezek és virtuális merevlemezek ismertetése](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4f916-101">For more information about disks, see [About Disks and VHDs for Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a><span data-ttu-id="4f916-102">Üres lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="4f916-102">Attach an empty disk</span></span>
1. <span data-ttu-id="4f916-103">Nyissa meg az Azure CLI 1.0 és [csatlakozás Azure-előfizetés tooyour](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="4f916-103">Open Azure CLI 1.0 and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="4f916-104">Bizonyosodjon meg róla, Azure szolgáltatásfelügyelet módban van-e (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="4f916-104">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="4f916-105">Adja meg `azure vm disk attach-new` toocreate és egy új lemezt csatolni a hello a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="4f916-105">Enter `azure vm disk attach-new` toocreate and attach a new disk as shown in hello following example.</span></span> <span data-ttu-id="4f916-106">Cserélje le *myVM* hello nevű a Linux virtuális gép, és adja meg a hello lemez hello méretét GB, ami *100GB* ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="4f916-106">Replace *myVM* with hello name of your Linux Virtual Machine and specify hello size of hello disk in GB, which is *100GB* in this example:</span></span>

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. <span data-ttu-id="4f916-107">Hello adatlemez létrehozni és csatolni, után hello kimenete felsorolt `azure vm disk list <virtual-machine-name>` a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="4f916-107">After hello data disk is created and attached, it's listed in hello output of `azure vm disk list <virtual-machine-name>` as shown in hello following example:</span></span>
   
    ```azurecli
    azure vm disk list TestVM
    ```

    <span data-ttu-id="4f916-108">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="4f916-108">hello output is similar toohello following example:</span></span>

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a><span data-ttu-id="4f916-109">Meglévő lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="4f916-109">Attach an existing disk</span></span>
<span data-ttu-id="4f916-110">Meglévő lemez csatlakoztatása esetén rendelkeznie kell egy tárfiókban elérhető .vhd-vel.</span><span class="sxs-lookup"><span data-stu-id="4f916-110">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span>

1. <span data-ttu-id="4f916-111">Nyissa meg az Azure CLI 1.0 és [csatlakozás Azure-előfizetés tooyour](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="4f916-111">Open Azure CLI 1.0 and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="4f916-112">Bizonyosodjon meg róla, Azure szolgáltatásfelügyelet módban van-e (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="4f916-112">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="4f916-113">Ellenőrizze, hogy ha hello VHD kívánt tooattach már feltöltött tooyour Azure-előfizetés:</span><span class="sxs-lookup"><span data-stu-id="4f916-113">Check if hello VHD you want tooattach is already uploaded tooyour Azure subscription:</span></span>
   
    ```azurecli
    azure vm disk list
    ```

    <span data-ttu-id="4f916-114">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="4f916-114">hello output is similar toohello following example:</span></span>

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. <span data-ttu-id="4f916-115">Ha nem találja a hello lemez adott meg toouse, előfordulhat, hogy töltse fel a helyi virtuális merevlemez tooyour előfizetés használatával `azure vm disk create` vagy `azure vm disk upload`.</span><span class="sxs-lookup"><span data-stu-id="4f916-115">If you don't find hello disk that you want toouse, you may upload a local VHD tooyour subscription by using `azure vm disk create` or `azure vm disk upload`.</span></span> <span data-ttu-id="4f916-116">Példa `disk create` lenne, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="4f916-116">An example of `disk create` would be as in hello following example:</span></span>
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    <span data-ttu-id="4f916-117">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="4f916-117">hello output is similar toohello following example:</span></span>

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   <span data-ttu-id="4f916-118">Is használhatja `azure vm disk upload` tooupload egy virtuális merevlemez tooa bizonyos tárolási fiókot.</span><span class="sxs-lookup"><span data-stu-id="4f916-118">You may also use `azure vm disk upload` tooupload a VHD tooa specific storage account.</span></span> <span data-ttu-id="4f916-119">Olvasási hello kapcsolatos parancsok toomanage az Azure virtuális gép adatlemezek [keresztül Itt](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="4f916-119">Read more about hello commands toomanage your Azure virtual machine data disks [over here](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

4. <span data-ttu-id="4f916-120">Most csatlakoztatása hello szükséges VHD tooyour virtuális gépet:</span><span class="sxs-lookup"><span data-stu-id="4f916-120">Now you attach hello desired VHD tooyour virtual machine:</span></span>
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   <span data-ttu-id="4f916-121">Győződjön meg arról, hogy tooreplace *myVM* hello nevet, a virtuális gépet, és *myVHD* rendelkező a kívánt virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="4f916-121">Make sure tooreplace *myVM* with hello name of your virtual machine, and *myVHD* with your desired VHD.</span></span>

5. <span data-ttu-id="4f916-122">Ellenőrizheti hello lemez virtuális géphez csatolt toohello `azure vm disk list <virtual-machine-name>`:</span><span class="sxs-lookup"><span data-stu-id="4f916-122">You can verify hello disk is attached toohello virtual machine with `azure vm disk list <virtual-machine-name>`:</span></span>
   
    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="4f916-123">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="4f916-123">hello output is similar toohello following example:</span></span>

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> <span data-ttu-id="4f916-124">Adatlemez hozzáadása után kell toolog toohello virtuális gépen kell és hello lemez inicializálása, hello virtuális gépek által használható hello lemez tárolás (lásd a következő hello lépéseit további információt hogyan toodo inicializálása hello lemez).</span><span class="sxs-lookup"><span data-stu-id="4f916-124">After you add a data disk, you'll need toolog on toohello virtual machine and initialize hello disk so hello virtual machine can use hello disk for storage (see hello following steps for more information on how toodo initialize hello disk).</span></span>
> 
> 

