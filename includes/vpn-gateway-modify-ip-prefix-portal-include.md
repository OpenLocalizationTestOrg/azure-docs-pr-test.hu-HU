### <span data-ttu-id="ecfca-101"><a name="noconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - átjáró kapcsolat</span><span class="sxs-lookup"><span data-stu-id="ecfca-101"><a name="noconnection"></a>toomodify local network gateway IP address prefixes - no gateway connection</span></span>

#### <a name="tooadd-additional-address-prefixes"></a><span data-ttu-id="ecfca-102">tooadd további címelőtagokat:</span><span class="sxs-lookup"><span data-stu-id="ecfca-102">tooadd additional address prefixes:</span></span>

1. <span data-ttu-id="ecfca-103">A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-103">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="ecfca-104">Hello IP-címterület hozzáadása a hello *újabb címtartomány felvétele* mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ecfca-104">Add hello IP address space in hello *Add additional address range* box.</span></span>
3. <span data-ttu-id="ecfca-105">Kattintson a **mentése** toosave a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ecfca-105">Click **Save** toosave your settings.</span></span>

#### <a name="tooremove-address-prefixes"></a><span data-ttu-id="ecfca-106">tooremove címelőtagok:</span><span class="sxs-lookup"><span data-stu-id="ecfca-106">tooremove address prefixes:</span></span>

1. <span data-ttu-id="ecfca-107">A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-107">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="ecfca-108">Kattintson a hello **"..."** hello előtag tartalmazó hello sor tooremove szeretné.</span><span class="sxs-lookup"><span data-stu-id="ecfca-108">Click hello **'...'** on hello line containing hello prefix you want tooremove.</span></span>
3. <span data-ttu-id="ecfca-109">Kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-109">Click **Remove**.</span></span>
4. <span data-ttu-id="ecfca-110">Kattintson a **mentése** toosave a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ecfca-110">Click **Save** toosave your settings.</span></span>

### <span data-ttu-id="ecfca-111"><a name="withconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - meglévő gateway-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="ecfca-111"><a name="withconnection"></a>toomodify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="ecfca-112">Ha átjáró kapcsolat és szeretné, hogy tooadd, vagy távolítsa el a hello a helyi hálózati átjáróban tárolt IP-címelőtagokat, akkor toodo hello lépések sorrendben.</span><span class="sxs-lookup"><span data-stu-id="ecfca-112">If you have a gateway connection and want tooadd or remove hello IP address prefixes contained in your local network gateway, you need toodo hello following steps, in order.</span></span> <span data-ttu-id="ecfca-113">Ez némi állásidőt jelent a VPN-kapcsolata számára.</span><span class="sxs-lookup"><span data-stu-id="ecfca-113">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="ecfca-114">Ha módosítja az IP-cím előtagokat, nincs szükség a toodelete hello VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="ecfca-114">When modifying IP address prefixes, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="ecfca-115">Csak akkor kell tooremove hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ecfca-115">You only need tooremove hello connection.</span></span>

#### <a name="1-remove-hello-connection"></a><span data-ttu-id="ecfca-116">1. Távolítsa el a hello kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ecfca-116">1. Remove hello connection.</span></span>

1. <span data-ttu-id="ecfca-117">A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-117">On hello Local Network Gateway resource, in hello **Settings** section, click **Connections**.</span></span>
2. <span data-ttu-id="ecfca-118">Kattintson a hello **...**  hello sor minden egyes kapcsolathoz, majd kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-118">Click hello **...** on hello line for each connection, then click **Delete**.</span></span>
3. <span data-ttu-id="ecfca-119">Kattintson a **mentése** toosave a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ecfca-119">Click **Save** toosave your settings.</span></span>

#### <a name="2-modify-hello-address-prefixes"></a><span data-ttu-id="ecfca-120">2. Módosítsa a címelőtagokat hello.</span><span class="sxs-lookup"><span data-stu-id="ecfca-120">2. Modify hello address prefixes.</span></span>

<span data-ttu-id="ecfca-121">tooadd további címelőtagokat:</span><span class="sxs-lookup"><span data-stu-id="ecfca-121">tooadd additional address prefixes:</span></span>

1. <span data-ttu-id="ecfca-122">A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-122">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="ecfca-123">Hello IP-címterület hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="ecfca-123">Add hello IP address space.</span></span>
3. <span data-ttu-id="ecfca-124">Kattintson a **mentése** toosave a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ecfca-124">Click **Save** toosave your settings.</span></span>

<span data-ttu-id="ecfca-125">tooremove címelőtagok:</span><span class="sxs-lookup"><span data-stu-id="ecfca-125">tooremove address prefixes:</span></span>

1. <span data-ttu-id="ecfca-126">A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-126">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="ecfca-127">Kattintson a hello **...**  hello előtag tartalmazó hello sor tooremove szeretné.</span><span class="sxs-lookup"><span data-stu-id="ecfca-127">Click hello **...** on hello line containing hello prefix you want tooremove.</span></span>
3. <span data-ttu-id="ecfca-128">Kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-128">Click **Remove**.</span></span>
4. <span data-ttu-id="ecfca-129">Kattintson a **mentése** toosave a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ecfca-129">Click **Save** toosave your settings.</span></span>

#### <a name="3-recreate-hello-connection"></a><span data-ttu-id="ecfca-130">3. Hozza létre újra a hello kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ecfca-130">3. Recreate hello connection.</span></span>

1. <span data-ttu-id="ecfca-131">Keresse meg a virtuális hálózati átjáró toohello a Vnethez tartozó.</span><span class="sxs-lookup"><span data-stu-id="ecfca-131">Navigate toohello Virtual Network Gateway for your VNet.</span></span> <span data-ttu-id="ecfca-132">(Már nem hello helyi hálózati átjáró van hátra.)</span><span class="sxs-lookup"><span data-stu-id="ecfca-132">(Not hello Local Network Gateway.)</span></span>
2. <span data-ttu-id="ecfca-133">A virtuális hálózati átjáró hello a hello **beállítások** kattintson **kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="ecfca-133">On hello Virtual Network Gateway, in hello **Settings** section, click **Connections**.</span></span>
3. <span data-ttu-id="ecfca-134">Kattintson a hello **+ Hozzáadás** tooopen hello **kapcsolat hozzáadása a** panelen.</span><span class="sxs-lookup"><span data-stu-id="ecfca-134">Click hello **+ Add** tooopen hello **Add connection** blade.</span></span>
4. <span data-ttu-id="ecfca-135">Hozza létre újra a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ecfca-135">Recreate your connection.</span></span>
5. <span data-ttu-id="ecfca-136">Kattintson a **OK** toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ecfca-136">Click **OK** toocreate hello connection.</span></span>
