<span data-ttu-id="ca746-101">Ha már nincs szüksége, amely virtuális géphez csatolt tooa adatlemezt, könnyen leválasztás.</span><span class="sxs-lookup"><span data-stu-id="ca746-101">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="ca746-102">A lemez leválasztása hello lemez eltávolítása a hello virtuális gépről, de nem hello lemez törlése hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="ca746-102">Detaching a disk removes hello disk from hello virtual machine, but doesn't delete hello disk from hello Azure storage account.</span></span>

<span data-ttu-id="ca746-103">Ha újra toouse hello meglévő hello a lemezen lévő adatokat, akkor is csatlakoztassa újra toohello ugyanahhoz a virtuális géphez, vagy egy másikra.</span><span class="sxs-lookup"><span data-stu-id="ca746-103">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="ca746-104">operációsrendszer-lemez toodetach, először toodelete hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ca746-104">toodetach an operating system disk, you first need toodelete hello virtual machine.</span></span>
>

## <a name="find-hello-disk"></a><span data-ttu-id="ca746-105">Hello lemez található</span><span class="sxs-lookup"><span data-stu-id="ca746-105">Find hello disk</span></span>
<span data-ttu-id="ca746-106">Ha nem tudja hello neve lemez hello vagy tooverify szeretné azt előtt válassza le azt, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ca746-106">If you don't know hello name of hello disk or want tooverify it before you detach it, follow these steps.</span></span>

1. <span data-ttu-id="ca746-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ca746-107">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="ca746-108">Kattintson a **virtuális gépek**, és ezután válasszon hello megfelelő méretű.</span><span class="sxs-lookup"><span data-stu-id="ca746-108">Click **Virtual Machines**, and then select hello appropriate VM.</span></span>

3. <span data-ttu-id="ca746-109">Kattintson a **lemezek** mentén hello bal szélétől hello virtuális gép irányítópult, a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ca746-109">Click **Disks** along hello left edge of hello virtual machine dashboard, under **Settings**.</span></span>

 <span data-ttu-id="ca746-110">hello virtuális gép irányítópult hello neve és a csatlakoztatott lemezek típusú sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="ca746-110">hello virtual machine dashboard lists hello name and type of all attached disks.</span></span> <span data-ttu-id="ca746-111">Ez a képernyő például egy virtuális gépet jelenít meg egy operációsrendszer-lemezzel és egy adatlemezzel:</span><span class="sxs-lookup"><span data-stu-id="ca746-111">For example, this screen shows a virtual machine with one operating system (OS) disk and one data disk:</span></span>

    ![Adatlemez megkeresése](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-hello-disk"></a><span data-ttu-id="ca746-113">Hello lemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="ca746-113">Detach hello disk</span></span>
1. <span data-ttu-id="ca746-114">A hello Azure-portálon, kattintson az **virtuális gépek**, majd kattintson a hello virtuális gépekről, amelyek azt szeretné, hogy toodetach hello adatlemez hello neve.</span><span class="sxs-lookup"><span data-stu-id="ca746-114">From hello Azure portal, click **Virtual Machines**, and then click hello name of hello virtual machine that has hello data disk you want toodetach.</span></span>

2. <span data-ttu-id="ca746-115">Kattintson a **lemezek** mentén hello bal szélétől hello virtuális gép irányítópult, a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ca746-115">Click **Disks** along hello left edge of hello virtual machine dashboard, under **Settings**.</span></span>

3. <span data-ttu-id="ca746-116">Kattintson a kívánt toodetach hello lemezre.</span><span class="sxs-lookup"><span data-stu-id="ca746-116">Click hello disk you want toodetach.</span></span>

  ![Hello lemez toodetach azonosítása](./media/howto-detach-disk-windows-linux/disklist.png)

4. <span data-ttu-id="ca746-118">Hello parancssávon kattintson **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="ca746-118">From hello command bar, click **Detach**.</span></span>

  ![Keresse meg a hello parancs leválasztása](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. <span data-ttu-id="ca746-120">Hello ablak, kattintson **Igen** toodetach hello lemez.</span><span class="sxs-lookup"><span data-stu-id="ca746-120">In hello confirmation window, click **Yes** toodetach hello disk.</span></span>

  ![Erősítse meg a hello lemez leválasztása](./media/howto-detach-disk-windows-linux/confirmdetach.png)

<span data-ttu-id="ca746-122">hello lemez tárolási megmarad, de már nem csatlakoztatott tooa virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ca746-122">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>
