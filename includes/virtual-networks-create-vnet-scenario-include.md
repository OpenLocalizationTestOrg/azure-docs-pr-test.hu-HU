## <a name="scenario"></a><span data-ttu-id="09b5a-101">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="09b5a-101">Scenario</span></span>
<span data-ttu-id="09b5a-102">toobetter bemutatják, hogyan toocreate egy VNet és alhálózat, ez a dokumentum használja az alábbi hello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="09b5a-102">toobetter illustrate how toocreate a VNet and subnets, this document will use hello scenario below.</span></span>

![VNet-forgatókönyv](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

<span data-ttu-id="09b5a-104">Ebben az esetben egy **TestVNet** nevű VNetet fog létrehozni a**192.168.0.0./16** egy fenntartott CIDR-blokkjával.</span><span class="sxs-lookup"><span data-stu-id="09b5a-104">In this scenario you will create a VNet named **TestVNet** with a reserved CIDR block of **192.168.0.0./16**.</span></span> <span data-ttu-id="09b5a-105">A virtuális hálózat hello a következő alhálózatokat fogja tartalmazni:</span><span class="sxs-lookup"><span data-stu-id="09b5a-105">Your VNet will contain hello following subnets:</span></span> 

* <span data-ttu-id="09b5a-106">**FrontEnd**, amelynek a CIDR-blokkja **192.168.1.0/24** lesz.</span><span class="sxs-lookup"><span data-stu-id="09b5a-106">**FrontEnd**, using **192.168.1.0/24** as its CIDR block.</span></span>
* <span data-ttu-id="09b5a-107">**BackEnd**, amelynek a CIDR-blokkja **192.168.2.0/24** lesz.</span><span class="sxs-lookup"><span data-stu-id="09b5a-107">**BackEnd**, using **192.168.2.0/24** as its CIDR block.</span></span>

