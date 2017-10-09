## <a name="how-toocreate-a-classic-vnet-using-azure-cli"></a><span data-ttu-id="6996e-101">Hogyan toocreate egy klasszikus virtuális hálózatot az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="6996e-101">How toocreate a classic VNet using Azure CLI</span></span>
<span data-ttu-id="6996e-102">Hello Azure CLI toomanage használhatja az Azure-erőforrások bármely olyan számítógépről, a Windows, Linux vagy os x rendszerű hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="6996e-102">You can use hello Azure CLI toomanage your Azure resources from hello command prompt from any computer running Windows, Linux, or OSX.</span></span> <span data-ttu-id="6996e-103">egy VNet hello Azure parancssori felület használatával toocreate kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6996e-103">toocreate a VNet by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="6996e-104">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../articles/cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="6996e-104">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../articles/cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="6996e-105">Futtassa a hello **azure-hálózat virtuális hálózat létrehozása** parancs toocreate egy VNet és alhálózat, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="6996e-105">Run hello **azure network vnet create** command toocreate a VNet and a subnet, as shown below.</span></span> <span data-ttu-id="6996e-106">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6996e-106">hello list shown after hello output explains hello parameters used.</span></span>
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    <span data-ttu-id="6996e-107">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6996e-107">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * <span data-ttu-id="6996e-108">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="6996e-108">**--vnet**.</span></span> <span data-ttu-id="6996e-109">Hello VNet toobe létrehozott neve.</span><span class="sxs-lookup"><span data-stu-id="6996e-109">Name of hello VNet toobe created.</span></span> <span data-ttu-id="6996e-110">A mi esetünkben *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="6996e-110">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="6996e-111">**-e (vagy--címtartomány)**.</span><span class="sxs-lookup"><span data-stu-id="6996e-111">**-e (or --address-space)**.</span></span> <span data-ttu-id="6996e-112">Virtuális hálózat címterület.</span><span class="sxs-lookup"><span data-stu-id="6996e-112">VNet address space.</span></span> <span data-ttu-id="6996e-113">A mi esetünkben *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="6996e-113">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="6996e-114">**-i (vagy - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="6996e-114">**-i (or -cidr)**.</span></span> <span data-ttu-id="6996e-115">Hálózati maszk CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="6996e-115">Network mask in CIDR format.</span></span> <span data-ttu-id="6996e-116">A mi esetünkben *16*.</span><span class="sxs-lookup"><span data-stu-id="6996e-116">For our scenario, *16*.</span></span>
   * <span data-ttu-id="6996e-117">**-n (vagy--alhálózatneve**).</span><span class="sxs-lookup"><span data-stu-id="6996e-117">**-n (or --subnet-name**).</span></span> <span data-ttu-id="6996e-118">Hello első alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="6996e-118">Name of hello first subnet.</span></span> <span data-ttu-id="6996e-119">A mi esetünkben *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="6996e-119">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="6996e-120">**-p (vagy--ip-alhálózat-start)**.</span><span class="sxs-lookup"><span data-stu-id="6996e-120">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="6996e-121">Kezdő IP-cím alhálózati vagy alhálózati címtartományt.</span><span class="sxs-lookup"><span data-stu-id="6996e-121">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="6996e-122">A mi esetünkben *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="6996e-122">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="6996e-123">**-r (vagy--alhálózat-cidr)**.</span><span class="sxs-lookup"><span data-stu-id="6996e-123">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="6996e-124">Hálózati maszk alhálózat CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="6996e-124">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="6996e-125">A mi esetünkben *24*.</span><span class="sxs-lookup"><span data-stu-id="6996e-125">For our scenario, *24*.</span></span>
   * <span data-ttu-id="6996e-126">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="6996e-126">**-l (or --location)**.</span></span> <span data-ttu-id="6996e-127">Azure-régió, ahol hello VNet létrejön.</span><span class="sxs-lookup"><span data-stu-id="6996e-127">Azure region where hello VNet will be created.</span></span> <span data-ttu-id="6996e-128">A mi esetünkben *USA középső RÉGIÓJA*.</span><span class="sxs-lookup"><span data-stu-id="6996e-128">For our scenario, *Central US*.</span></span>
3. <span data-ttu-id="6996e-129">Futtassa a hello **azure vnet alhálózaton létrehozása** parancs toocreate alhálózat alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="6996e-129">Run hello **azure network vnet subnet create** command toocreate a subnet as shown below.</span></span> <span data-ttu-id="6996e-130">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6996e-130">hello list shown after hello output explains hello parameters used.</span></span>
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    <span data-ttu-id="6996e-131">Itt egy hello parancs fenti hello várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6996e-131">Here is hello expected output for hello command above:</span></span>
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * <span data-ttu-id="6996e-132">**-t (vagy--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="6996e-132">**-t (or --vnet-name**.</span></span> <span data-ttu-id="6996e-133">Hello ahol hello alhálózati létrejön a VNet neve.</span><span class="sxs-lookup"><span data-stu-id="6996e-133">Name of hello VNet where hello subnet will be created.</span></span> <span data-ttu-id="6996e-134">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="6996e-134">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="6996e-135">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="6996e-135">**-n (or --name)**.</span></span> <span data-ttu-id="6996e-136">Hello új alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="6996e-136">Name of hello new subnet.</span></span> <span data-ttu-id="6996e-137">A mi esetünkben *háttér*.</span><span class="sxs-lookup"><span data-stu-id="6996e-137">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="6996e-138">**-a (vagy --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="6996e-138">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="6996e-139">Az alhálózat CIDR-blokkja.</span><span class="sxs-lookup"><span data-stu-id="6996e-139">Subnet CIDR block.</span></span> <span data-ttu-id="6996e-140">Négy esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="6996e-140">Four our scenario, *192.168.2.0/24*.</span></span>
4. <span data-ttu-id="6996e-141">Futtassa a hello **azure hálózati vnet show** tooview hello hello új vnet tulajdonságainak parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="6996e-141">Run hello **azure network vnet show** command tooview hello properties of hello new vnet, as shown below.</span></span>
   
            azure network vnet show
   
    <span data-ttu-id="6996e-142">Itt egy hello parancs fenti hello várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6996e-142">Here is hello expected output for hello command above:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
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

