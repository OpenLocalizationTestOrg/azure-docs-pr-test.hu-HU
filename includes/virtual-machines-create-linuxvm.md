
1. <span data-ttu-id="d332f-101">Jelentkezzen be tooyour hello lépésekkel felsorolt Azure-előfizetés [tooAzure csatlakoztatja a hello Azure CLI 1.0](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="d332f-101">Sign in tooyour Azure subscription using hello steps listed in [Connect tooAzure from hello Azure CLI 1.0](../articles/xplat-cli-connect.md).</span></span>

2. <span data-ttu-id="d332f-102">Gondoskodjon arról, hogy Ön hello klasszikus telepítési módban az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d332f-102">Make sure you are in hello Classic deployment mode as follows:</span></span>

    ```azurecli
    azure config mode asm
    ```

3. <span data-ttu-id="d332f-103">Megtudhatja, hello Linux lemezképet, amelyet a rendelkezésre álló lemezképeinek hello tooload az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d332f-103">Find out hello Linux image that you want tooload from hello available images as follows:</span></span>

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    <span data-ttu-id="d332f-104">Windows-parancsablakokban a grep helyett használja a **find** kifejezést.</span><span class="sxs-lookup"><span data-stu-id="d332f-104">In a Windows command-prompt window, use **find** instead of grep.</span></span>
   
4. <span data-ttu-id="d332f-105">Használjon `azure vm create` toocreate hello Linux lemezképpel hello előző listában egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="d332f-105">Use `azure vm create` toocreate a VM with hello Linux image from hello previous list.</span></span> <span data-ttu-id="d332f-106">Ezzel a lépéssel létrehoz egy felhőszolgáltatást és egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d332f-106">This step creates a cloud service and storage account.</span></span> <span data-ttu-id="d332f-107">A virtuális gép tooan meglévő felhőszolgáltatás is kapcsolódhat egy `-c` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d332f-107">You could also connect this VM tooan existing cloud service with a `-c` option.</span></span> <span data-ttu-id="d332f-108">Hozzon létre egy SSH-végpont toolog toohello Linux virtuális gép hello `-e` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d332f-108">Create an SSH endpoint toolog in toohello Linux virtual machine with hello `-e` option.</span></span> <span data-ttu-id="d332f-109">hello alábbi példa létrehoz egy nevű virtuális gép `myVM` hello segítségével `Ubuntu-14_04_4-LTS` hello lemezképet `West US` helyét, és hozzáad egy felhasználói nevet `ops`:</span><span class="sxs-lookup"><span data-stu-id="d332f-109">hello following example creates a VM named `myVM` using hello `Ubuntu-14_04_4-LTS` image in hello `West US` location, and adds a user name `ops`:</span></span>
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    <span data-ttu-id="d332f-110">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="d332f-110">hello output is similar toohello following example:</span></span>

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > <span data-ttu-id="d332f-111">A Linux virtuális gép esetén meg kell adnia hello `-e` beállítást `vm create`.</span><span class="sxs-lookup"><span data-stu-id="d332f-111">For a Linux virtual machine, you must provide hello `-e` option in `vm create`.</span></span> <span data-ttu-id="d332f-112">Nincs SSH lehetséges tooenable hello virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="d332f-112">It is not possible tooenable SSH after hello virtual machine has been created.</span></span> <span data-ttu-id="d332f-113">Az SSH további tudnivalókért olvassa el [hogyan tooUse Azure Linux SSH](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d332f-113">For more details on SSH, read [How tooUse SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

5. <span data-ttu-id="d332f-114">Hello segítségével ellenőrizheti a hello VM hello attribútumait `azure vm show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d332f-114">You can verify hello attributes of hello VM by using hello `azure vm show` command.</span></span> <span data-ttu-id="d332f-115">hello alábbi példa felsorolja hello nevű virtuális gép adatai `myVM`:</span><span class="sxs-lookup"><span data-stu-id="d332f-115">hello following example lists information for hello VM named `myVM`:</span></span>

    ```azurecli   
    azure vm show myVM
    ```

6. <span data-ttu-id="d332f-116">Indítsa el a virtuális Gépet a hello `azure vm start` parancsot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="d332f-116">Start your VM with hello `azure vm start` command as follows:</span></span>

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="d332f-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d332f-117">Next steps</span></span>
<span data-ttu-id="d332f-118">Ezek Azure CLI 1.0 virtuális gép parancsok tudnivalókért olvassa el a hello [Using hello Azure CLI 1.0 hello klasszikus telepítés API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="d332f-118">For details on all these Azure CLI 1.0 virtual machine commands, read hello [Using hello Azure CLI 1.0 with hello Classic deployment API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

