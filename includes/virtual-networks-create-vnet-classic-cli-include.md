## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a><span data-ttu-id="ff64c-101">Egy Azure parancssori felület használatával klasszikus virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff64c-101">How to create a classic VNet using Azure CLI</span></span>
<span data-ttu-id="ff64c-102">Az Azure CLI segítségével a parancssorból felügyelheti az erőforrásokat bármilyen Windows, Linux vagy OSX rendszert futtató számítógépről.</span><span class="sxs-lookup"><span data-stu-id="ff64c-102">You can use the Azure CLI to manage your Azure resources from the command prompt from any computer running Windows, Linux, or OSX.</span></span> <span data-ttu-id="ff64c-103">Az alábbi lépésekkel hozhat létre egy VNetet az Azure CLI segítségével.</span><span class="sxs-lookup"><span data-stu-id="ff64c-103">To create a VNet by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="ff64c-104">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Install and Configure the Azure CLI](../articles/cli-install-nodejs.md) (Az Azure CLI telepítése és konfigurálása) részt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="ff64c-104">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../articles/cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="ff64c-105">Futtassa az **azure network vnet create** parancsot egy VNet és egy alhálózat létrehozásához az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="ff64c-105">Run the **azure network vnet create** command to create a VNet and a subnet, as shown below.</span></span> <span data-ttu-id="ff64c-106">A kimenet után látható lista ismerteti a használt paramétereket.</span><span class="sxs-lookup"><span data-stu-id="ff64c-106">The list shown after the output explains the parameters used.</span></span>
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    <span data-ttu-id="ff64c-107">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="ff64c-107">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * <span data-ttu-id="ff64c-108">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-108">**--vnet**.</span></span> <span data-ttu-id="ff64c-109">A létrehozni kívánt VNet neve.</span><span class="sxs-lookup"><span data-stu-id="ff64c-109">Name of the VNet to be created.</span></span> <span data-ttu-id="ff64c-110">A mi esetünkben *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="ff64c-110">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="ff64c-111">**-e (vagy--címtartomány)**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-111">**-e (or --address-space)**.</span></span> <span data-ttu-id="ff64c-112">Virtuális hálózat címterület.</span><span class="sxs-lookup"><span data-stu-id="ff64c-112">VNet address space.</span></span> <span data-ttu-id="ff64c-113">A mi esetünkben *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="ff64c-113">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="ff64c-114">**-i (vagy - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-114">**-i (or -cidr)**.</span></span> <span data-ttu-id="ff64c-115">Hálózati maszk CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="ff64c-115">Network mask in CIDR format.</span></span> <span data-ttu-id="ff64c-116">A mi esetünkben *16*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-116">For our scenario, *16*.</span></span>
   * <span data-ttu-id="ff64c-117">**-n (vagy--alhálózatneve**).</span><span class="sxs-lookup"><span data-stu-id="ff64c-117">**-n (or --subnet-name**).</span></span> <span data-ttu-id="ff64c-118">Az első alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="ff64c-118">Name of the first subnet.</span></span> <span data-ttu-id="ff64c-119">A mi esetünkben *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-119">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="ff64c-120">**-p (vagy--ip-alhálózat-start)**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-120">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="ff64c-121">Kezdő IP-cím alhálózati vagy alhálózati címtartományt.</span><span class="sxs-lookup"><span data-stu-id="ff64c-121">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="ff64c-122">A mi esetünkben *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-122">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="ff64c-123">**-r (vagy--alhálózat-cidr)**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-123">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="ff64c-124">Hálózati maszk alhálózat CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="ff64c-124">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="ff64c-125">A mi esetünkben *24*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-125">For our scenario, *24*.</span></span>
   * <span data-ttu-id="ff64c-126">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-126">**-l (or --location)**.</span></span> <span data-ttu-id="ff64c-127">Az Azure-régió, ahol a VNet létrejön.</span><span class="sxs-lookup"><span data-stu-id="ff64c-127">Azure region where the VNet will be created.</span></span> <span data-ttu-id="ff64c-128">A mi esetünkben *USA középső RÉGIÓJA*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-128">For our scenario, *Central US*.</span></span>
3. <span data-ttu-id="ff64c-129">Futtassa az **azure network vnet subnet create** parancsot egy alhálózat létrehozásához az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="ff64c-129">Run the **azure network vnet subnet create** command to create a subnet as shown below.</span></span> <span data-ttu-id="ff64c-130">A kimenet után látható lista ismerteti a használt paramétereket.</span><span class="sxs-lookup"><span data-stu-id="ff64c-130">The list shown after the output explains the parameters used.</span></span>
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    <span data-ttu-id="ff64c-131">A fenti parancs várható kimenete:</span><span class="sxs-lookup"><span data-stu-id="ff64c-131">Here is the expected output for the command above:</span></span>
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * <span data-ttu-id="ff64c-132">**-t (vagy--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-132">**-t (or --vnet-name**.</span></span> <span data-ttu-id="ff64c-133">A VNet neve, ahol létrejön az alhálózat.</span><span class="sxs-lookup"><span data-stu-id="ff64c-133">Name of the VNet where the subnet will be created.</span></span> <span data-ttu-id="ff64c-134">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-134">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="ff64c-135">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-135">**-n (or --name)**.</span></span> <span data-ttu-id="ff64c-136">Az új alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="ff64c-136">Name of the new subnet.</span></span> <span data-ttu-id="ff64c-137">A mi esetünkben *háttér*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-137">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="ff64c-138">**-a (vagy --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="ff64c-138">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="ff64c-139">Az alhálózat CIDR-blokkja.</span><span class="sxs-lookup"><span data-stu-id="ff64c-139">Subnet CIDR block.</span></span> <span data-ttu-id="ff64c-140">Négy esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="ff64c-140">Four our scenario, *192.168.2.0/24*.</span></span>
4. <span data-ttu-id="ff64c-141">Futtassa az **azure network vnet show** parancsot az új vnet tulajdonságainak megtekintéséhez a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="ff64c-141">Run the **azure network vnet show** command to view the properties of the new vnet, as shown below.</span></span>
   
            azure network vnet show
   
    <span data-ttu-id="ff64c-142">A fenti parancs várható kimenete:</span><span class="sxs-lookup"><span data-stu-id="ff64c-142">Here is the expected output for the command above:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

