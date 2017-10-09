<span data-ttu-id="6cb3a-101">Ha már nincs szüksége, amely csatolt tooa virtuális gép (VM) adatlemezt, könnyen leválasztás.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-101">When you no longer need a data disk that's attached tooa virtual machine (VM), you can easily detach it.</span></span> <span data-ttu-id="6cb3a-102">Ha Ön a lemezt leválasztani a virtuális gép hello, hello lemez nem raktárból azt.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-102">When you detach a disk from hello VM, hello disk is not removed it from storage.</span></span> <span data-ttu-id="6cb3a-103">Ha újra toouse hello meglévő hello a lemezen lévő adatokat, akkor is csatlakoztassa újra toohello azonos virtuális gép vagy egy másikra.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-103">If you want toouse hello existing data on hello disk again, you can reattach it toohello same VM, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="6cb3a-104">Az Azure-ban a virtuális gépek különböző lemeztípusokat használnak – operációsrendszer-lemezt, helyi ideiglenes lemezt és opcionális adatlemezeket.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-104">A VM in Azure uses different types of disks - an operating system disk, a local temporary disk, and optional data disks.</span></span> <span data-ttu-id="6cb3a-105">További információ: [A lemezek és virtuális merevlemezek ismertetése](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6cb3a-105">For details, see [About Disks and VHDs for Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="6cb3a-106">Operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép hello is törli.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-106">You cannot detach an operating system disk unless you also delete hello VM.</span></span>

## <a name="find-hello-disk"></a><span data-ttu-id="6cb3a-107">Hello lemez található</span><span class="sxs-lookup"><span data-stu-id="6cb3a-107">Find hello disk</span></span>
<span data-ttu-id="6cb3a-108">Akkor is a lemezt leválasztani a virtuális gép kell toofind kimenő hello logikai egység száma, amely hello lemez toobe leválasztott azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-108">Before you can detach a disk from a VM you need toofind out hello LUN number, which is an identifier for hello disk toobe detached.</span></span> <span data-ttu-id="6cb3a-109">az toodo, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-109">toodo that, follow these steps:</span></span>

1. <span data-ttu-id="6cb3a-110">Nyissa meg az Azure CLI és [csatlakozás Azure-előfizetés tooyour](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="6cb3a-110">Open Azure CLI and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="6cb3a-111">Bizonyosodjon meg róla, Azure szolgáltatásfelügyelet módban van-e (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="6cb3a-111">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="6cb3a-112">Annak meghatározása, hogy mely lemezek csatolt tooyour VM-et</span><span class="sxs-lookup"><span data-stu-id="6cb3a-112">Find out which disks are attached tooyour VM.</span></span> <span data-ttu-id="6cb3a-113">hello alábbi példa felsorolja hello nevű virtuális gép lemezeinek `myVM`:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-113">hello following example lists disks for hello VM named `myVM`:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="6cb3a-114">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-114">hello output is similar toohello following example:</span></span>

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. <span data-ttu-id="6cb3a-115">Vegye figyelembe a logikai egység hello vagy hello **logikaiegység-szám** , amelyet az toodetach hello lemezhez.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-115">Note hello LUN or hello **logical unit number** for hello disk that you want toodetach.</span></span>

## <a name="remove-operating-system-references-toohello-disk"></a><span data-ttu-id="6cb3a-116">Operációs rendszer hivatkozások toohello lemez eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6cb3a-116">Remove operating system references toohello disk</span></span>
<span data-ttu-id="6cb3a-117">Mielőtt hello lemez leválasztása a hello Linux Vendég, meg kell győződnie arról, hogy minden partíció hello lemezen nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-117">Before detaching hello disk from hello Linux guest, you should make sure that all partitions on hello disk are not in use.</span></span> <span data-ttu-id="6cb3a-118">Győződjön meg arról, hogy hello operációs rendszer nem próbálkozik tooremount őket a rendszer újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-118">Ensure that hello operating system does not attempt tooremount them after a reboot.</span></span> <span data-ttu-id="6cb3a-119">Ezeket a lépéseket a visszavonás vélhetőleg létrehozta azon mikor hello konfigurációs [csatolása](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello lemez.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-119">These steps undo hello configuration you likely created when [attaching](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello disk.</span></span>

1. <span data-ttu-id="6cb3a-120">Használjon hello `lsscsi` parancs toodiscover hello lemez azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-120">Use hello `lsscsi` command toodiscover hello disk identifier.</span></span> <span data-ttu-id="6cb3a-121">Az `lsscsi` a `yum install lsscsi` (Red Hat-alapú disztribúciókon) vagy az `apt-get install lsscsi` (Debian-alapú disztribúciókon) használatával telepíthető.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-121">`lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="6cb3a-122">Hello LUN számot a keresett hello lemezazonosítót található.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-122">You can find hello disk identifier you are looking for by using hello LUN number.</span></span> <span data-ttu-id="6cb3a-123">hello utolsó hello rekordot sorokban értéke hello logikai Egységet.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-123">hello last number in hello tuple in each row is hello LUN.</span></span> <span data-ttu-id="6cb3a-124">Az alábbi példa hello `lsscsi`, LUN 0 leképezhető túl  */dev/sdc*</span><span class="sxs-lookup"><span data-stu-id="6cb3a-124">In hello following example from `lsscsi`, LUN 0 maps too*/dev/sdc*</span></span>

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. <span data-ttu-id="6cb3a-125">Használjon `fdisk -l <disk>` hello lemez toobe leválasztott társított toodiscover hello partíciókat.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-125">Use `fdisk -l <disk>` toodiscover hello partitions associated with hello disk toobe detached.</span></span> <span data-ttu-id="6cb3a-126">hello következő példa bemutatja a hello kimeneti `/dev/sdc`:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-126">hello following example shows hello output for `/dev/sdc`:</span></span>

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. <span data-ttu-id="6cb3a-127">Mindegyik partíció felsorolt hello lemez leválasztása.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-127">Unmount each partition listed for hello disk.</span></span> <span data-ttu-id="6cb3a-128">hello alábbi példa leválasztja `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-128">hello following example unmounts `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

4. <span data-ttu-id="6cb3a-129">Használjon hello `blkid` parancs toodiscovery hello UUID-k minden partíció esetében.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-129">Use hello `blkid` command toodiscovery hello UUIDs for all partitions.</span></span> <span data-ttu-id="6cb3a-130">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-130">hello output is similar toohello following example:</span></span>

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. <span data-ttu-id="6cb3a-131">Távolítsa el a bejegyzéseket hello **/etc/fstab** tartozó hello eszköz elérési utak vagy UUID-k hello lemez toobe leválasztott tartozó összes partíció fájlt.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-131">Remove entries in hello **/etc/fstab** file associated with either hello device paths or UUIDs for all partitions for hello disk toobe detached.</span></span>  <span data-ttu-id="6cb3a-132">Ebben a példában a bejegyzések a következők lehetnek:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-132">Entries for this example might be:</span></span>

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    <span data-ttu-id="6cb3a-133">vagy</span><span class="sxs-lookup"><span data-stu-id="6cb3a-133">or</span></span>
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-hello-disk"></a><span data-ttu-id="6cb3a-134">Hello lemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="6cb3a-134">Detach hello disk</span></span>
<span data-ttu-id="6cb3a-135">Hello logikai egység száma hello lemez és eltávolított hello operációs rendszer hivatkozások megtalálta most készen áll a toodetach azt:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-135">After you find hello LUN number of hello disk and removed hello operating system references, you're ready toodetach it:</span></span>

1. <span data-ttu-id="6cb3a-136">Hello kijelölt lemezt leválasztani a virtuális gép hello hello parancs futtatásával `azure vm disk detach
   <virtual-machine-name> <LUN>`.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-136">Detach hello selected disk from hello virtual machine by running hello command `azure vm disk detach
<virtual-machine-name> <LUN>`.</span></span> <span data-ttu-id="6cb3a-137">hello alábbi példa leválasztja LUN `0` hello nevű virtuális gép a `myVM`:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-137">hello following example detaches LUN `0` from hello VM named `myVM`:</span></span>
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. <span data-ttu-id="6cb3a-138">Ha hello lemez leválasztott állapotban a kapott a következő futtatásával ellenőrizheti `azure vm disk list` újra.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-138">You can check if hello disk got detached by running `azure vm disk list` again.</span></span> <span data-ttu-id="6cb3a-139">a következő példa ellenőrzések hello hello nevű virtuális gép `myVM`:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-139">hello following example checks hello VM named `myVM`:</span></span>
   
    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="6cb3a-140">a kimeneti hello van a hasonló toohello következő példának, amely bemutatja a hello adatok lemez már csatlakoztatva van:</span><span class="sxs-lookup"><span data-stu-id="6cb3a-140">hello output is similar toohello following example, which shows hello data disk is no longer attached:</span></span>

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

<span data-ttu-id="6cb3a-141">lemez leválasztása hello tárolási megmarad, de már nem csatlakoztatott tooa virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6cb3a-141">hello detached disk remains in storage but is no longer attached tooa virtual machine.</span></span>

