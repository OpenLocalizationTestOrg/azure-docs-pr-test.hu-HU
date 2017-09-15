<span data-ttu-id="877af-101">Ha már nincs szüksége egy virtuális géphez csatolt adatlemezre, könnyedén leválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="877af-101">When you no longer need a data disk that's attached to a virtual machine, you can easily detach it.</span></span> <span data-ttu-id="877af-102">Ha leválaszt egy lemezt, azzal eltávolítja azt a virtuális gépről, de nem törli az Azure Storage-fiókból.</span><span class="sxs-lookup"><span data-stu-id="877af-102">Detaching a disk removes the disk from the virtual machine, but doesn't delete the disk from the Azure storage account.</span></span>

<span data-ttu-id="877af-103">Ha ismét használni szeretné a lemezen lévő adatokat, újból csatolhatja ugyanahhoz vagy egy másik virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="877af-103">If you want to use the existing data on the disk again, you can reattach it to the same virtual machine, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="877af-104">Az operációsrendszer-lemez leválasztásához előbb törölnie kell a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="877af-104">To detach an operating system disk, you first need to delete the virtual machine.</span></span>
>

## <a name="find-the-disk"></a><span data-ttu-id="877af-105">A lemez megkeresése</span><span class="sxs-lookup"><span data-stu-id="877af-105">Find the disk</span></span>
<span data-ttu-id="877af-106">Ha nem tudja a lemez nevét, vagy szeretné ellenőrizni a leválasztás előtt, kövesse a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="877af-106">If you don't know the name of the disk or want to verify it before you detach it, follow these steps.</span></span>

1. <span data-ttu-id="877af-107">Jelentkezzen be az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="877af-107">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="877af-108">Kattintson a **Virtual Machines** lehetőségre, majd válassza ki a megfelelő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="877af-108">Click **Virtual Machines**, and then select the appropriate VM.</span></span>

3. <span data-ttu-id="877af-109">Kattintson a virtuális gép irányítópultjának bal szélén, a **Beállítások** területen a **Lemezek** elemre.</span><span class="sxs-lookup"><span data-stu-id="877af-109">Click **Disks** along the left edge of the virtual machine dashboard, under **Settings**.</span></span>

 <span data-ttu-id="877af-110">A virtuális gép irányítópultja felsorolja a csatlakoztatott lemezek nevét és típusát.</span><span class="sxs-lookup"><span data-stu-id="877af-110">The virtual machine dashboard lists the name and type of all attached disks.</span></span> <span data-ttu-id="877af-111">Ez a képernyő például egy virtuális gépet jelenít meg egy operációsrendszer-lemezzel és egy adatlemezzel:</span><span class="sxs-lookup"><span data-stu-id="877af-111">For example, this screen shows a virtual machine with one operating system (OS) disk and one data disk:</span></span>

    ![Adatlemez megkeresése](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-the-disk"></a><span data-ttu-id="877af-113">A lemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="877af-113">Detach the disk</span></span>
1. <span data-ttu-id="877af-114">Az Azure Portalon kattintson a **Virtuális gépek** lehetőségre, majd kattintson annak a virtuális gépnek a nevére, amelyen a leválasztani kívánt adatok vannak.</span><span class="sxs-lookup"><span data-stu-id="877af-114">From the Azure portal, click **Virtual Machines**, and then click the name of the virtual machine that has the data disk you want to detach.</span></span>

2. <span data-ttu-id="877af-115">Kattintson a virtuális gép irányítópultjának bal szélén, a **Beállítások** területen a **Lemezek** elemre.</span><span class="sxs-lookup"><span data-stu-id="877af-115">Click **Disks** along the left edge of the virtual machine dashboard, under **Settings**.</span></span>

3. <span data-ttu-id="877af-116">Kattintson a leválasztani kívánt lemezre.</span><span class="sxs-lookup"><span data-stu-id="877af-116">Click the disk you want to detach.</span></span>

  ![A leválasztani kívánt lemez azonosítása](./media/howto-detach-disk-windows-linux/disklist.png)

4. <span data-ttu-id="877af-118">A parancssoron kattintson a **Leválasztás** elemre.</span><span class="sxs-lookup"><span data-stu-id="877af-118">From the command bar, click **Detach**.</span></span>

  ![A leválasztási parancs megkeresése](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. <span data-ttu-id="877af-120">A megerősítés ablakban kattintson az **Igen** gombra a lemez leválasztásához.</span><span class="sxs-lookup"><span data-stu-id="877af-120">In the confirmation window, click **Yes** to detach the disk.</span></span>

  ![A lemez leválasztásának megerősítése](./media/howto-detach-disk-windows-linux/confirmdetach.png)

<span data-ttu-id="877af-122">A lemez a tárolóban marad, de már nincs virtuális géphez csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="877af-122">The disk remains in storage but is no longer attached to a virtual machine.</span></span>
