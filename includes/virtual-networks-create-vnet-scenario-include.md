## <a name="scenario"></a><span data-ttu-id="f3f00-101">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="f3f00-101">Scenario</span></span>
<span data-ttu-id="f3f00-102">A VNetek létrehozásának jobb bemutatásához az alábbi forgatókönyvet használja majd a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="f3f00-102">To better illustrate how to create a VNet and subnets, this document will use the scenario below.</span></span>

![VNet-forgatókönyv](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

<span data-ttu-id="f3f00-104">Ebben az esetben egy **TestVNet** nevű VNetet fog létrehozni a**192.168.0.0./16** egy fenntartott CIDR-blokkjával.</span><span class="sxs-lookup"><span data-stu-id="f3f00-104">In this scenario you will create a VNet named **TestVNet** with a reserved CIDR block of **192.168.0.0./16**.</span></span> <span data-ttu-id="f3f00-105">A VNet a következő alhálózatokat fogja tartalmazni:</span><span class="sxs-lookup"><span data-stu-id="f3f00-105">Your VNet will contain the following subnets:</span></span> 

* <span data-ttu-id="f3f00-106">**FrontEnd**, amelynek a CIDR-blokkja **192.168.1.0/24** lesz.</span><span class="sxs-lookup"><span data-stu-id="f3f00-106">**FrontEnd**, using **192.168.1.0/24** as its CIDR block.</span></span>
* <span data-ttu-id="f3f00-107">**BackEnd**, amelynek a CIDR-blokkja **192.168.2.0/24** lesz.</span><span class="sxs-lookup"><span data-stu-id="f3f00-107">**BackEnd**, using **192.168.2.0/24** as its CIDR block.</span></span>

