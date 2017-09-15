

![Önálló virtuális gépek a felhőalapú szolgáltatás](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

<span data-ttu-id="0421b-102">Ha a virtuális gépeket helyez egy virtuális hálózatot, megadhatja, hogy a használni kívánt hány felhőszolgáltatások betölteni a terheléselosztás és a rendelkezésre állási készletek.</span><span class="sxs-lookup"><span data-stu-id="0421b-102">If you place your virtual machines in a virtual network, you can decide how many cloud services you want to use for load balancing and availability sets.</span></span> <span data-ttu-id="0421b-103">Emellett rendezheti a virtuális gépek alhálózatok ugyanúgy a helyszíni hálózat és a virtuális hálózathoz csatlakozni a helyszíni hálózat.</span><span class="sxs-lookup"><span data-stu-id="0421b-103">Additionally, you can organize the virtual machines on subnets in the same way as your on-premises network and connect the virtual network to your on-premises network.</span></span> <span data-ttu-id="0421b-104">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="0421b-104">Here's an example:</span></span>

![A virtuális hálózatban lévő virtuális gépek](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

<span data-ttu-id="0421b-106">Virtuális hálózatok az ajánlott módszer a virtuális gépek Azure-ban való csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="0421b-106">Virtual networks are the recommended way to connect virtual machines in Azure.</span></span> <span data-ttu-id="0421b-107">Az ajánlott eljárás az alkalmazás egyes rétegeinek konfigurálása külön felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="0421b-107">The best practice is to configure each tier of your application in a separate cloud service.</span></span> <span data-ttu-id="0421b-108">Azonban szükség lehet néhány virtuális gépnél, a másik alkalmazási rétegek egyesítése a azonos felhőszolgáltatás 200 felhőszolgáltatások előfizetésenként legfeljebb belül maradjon.</span><span class="sxs-lookup"><span data-stu-id="0421b-108">However, you may need to combine some virtual machines from different application tiers into the same cloud service to remain within the maximum of 200 cloud services per subscription.</span></span> <span data-ttu-id="0421b-109">Ezzel és más korlátozások ellenőrzéséhez tekintse meg [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../articles/azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="0421b-109">To review this and other limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../articles/azure-subscription-service-limits.md).</span></span>

## <a name="connect-vms-in-a-virtual-network"></a><span data-ttu-id="0421b-110">Csatlakozás a virtuális gépek virtuális hálózatban</span><span class="sxs-lookup"><span data-stu-id="0421b-110">Connect VMs in a virtual network</span></span>
<span data-ttu-id="0421b-111">A virtuális hálózatban lévő virtuális gépek csatlakozni:</span><span class="sxs-lookup"><span data-stu-id="0421b-111">To connect virtual machines in a virtual network:</span></span>

1. <span data-ttu-id="0421b-112">A virtuális hálózat létrehozása a [Azure-portálon](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) , és adja meg a "klasszikus üzembe helyezési".</span><span class="sxs-lookup"><span data-stu-id="0421b-112">Create the virtual network in the [Azure portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) and specify 'classic deployment'.</span></span>
2. <span data-ttu-id="0421b-113">Hozzon létre a felhőalapú szolgáltatások a környezetet, hogy tükrözze a tervező a rendelkezésre állási csoportok és a terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="0421b-113">Create the set of cloud services for your deployment to reflect your design for availability sets and load balancing.</span></span> <span data-ttu-id="0421b-114">Az Azure portálon kattintson **új > számítási > Felhőszolgáltatás** minden felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0421b-114">In the Azure portal, click **New > Compute > Cloud service** for each cloud service.</span></span>

  <span data-ttu-id="0421b-115">Mivel kitöltötte a felhőalapú szolgáltatás részleteit, válassza ki ugyanazt _erőforráscsoport_ együtt a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="0421b-115">As you fill out the cloud service details, choose the same _resource group_ used with the virtual network.</span></span>

3. <span data-ttu-id="0421b-116">Minden új virtuális gép létrehozásához kattintson a **új > számítási**, majd válassza ki a megfelelő Virtuálisgép-lemezkép a **a kiemelt alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0421b-116">To create each new virtual machine, click **New > Compute**, then select the appropriate VM image from the **Featured apps**.</span></span>

  <span data-ttu-id="0421b-117">A virtuális gép **alapjai** panelen válassza ki ugyanazt _erőforráscsoport_ együtt a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="0421b-117">In the VM **Basics** blade, choose the same _resource group_ used with the virtual network.</span></span>

  ![Virtuális gép alapvető beállítások panel egy virtuális hálózat használatakor](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. <span data-ttu-id="0421b-119">Adja meg a virtuális Gépet, **beállítások**, válassza ki a megfelelő _felhőalapú szolgáltatás_ vagy _virtuális hálózati_ a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="0421b-119">As you fill out the VM **Settings**, choose the correct _Cloud service_ or _virtual network_ for the VM.</span></span>

  <span data-ttu-id="0421b-120">Azure fogja kiválasztani az egyéb elem a kijelölés alapján.</span><span class="sxs-lookup"><span data-stu-id="0421b-120">Azure will select the other item based on your selection.</span></span>

  ![Virtuálisgép-beállítások panel egy virtuális hálózat használatakor](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a><span data-ttu-id="0421b-122">Csatlakozás a virtuális gépek önálló felhőszolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="0421b-122">Connect VMs in a standalone cloud service</span></span>
<span data-ttu-id="0421b-123">Önálló felhőalapú szolgáltatásként lévő virtuális gépek csatlakozni:</span><span class="sxs-lookup"><span data-stu-id="0421b-123">To connect virtual machines in a standalone cloud service:</span></span>

1. <span data-ttu-id="0421b-124">A felhőszolgáltatás létrehozása a [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0421b-124">Create the cloud service in the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="0421b-125">Kattintson a **új > számítási > Felhőszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="0421b-125">Click **New > Compute > Cloud service**.</span></span> <span data-ttu-id="0421b-126">Vagy a felhőalapú szolgáltatás, az üzembe helyezéshez hozhat létre, amikor az első virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0421b-126">Or, you can create the cloud service for your deployment when you create your first virtual machine.</span></span>
2. <span data-ttu-id="0421b-127">A virtuális gépek létrehozásakor válassza ki a felhőalapú szolgáltatással használt ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="0421b-127">When you create the virtual machines, choose the same resource group used with the cloud service.</span></span>

  ![A virtuális gépek hozzáadása egy meglévő felhőszolgáltatáshoz](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  <span data-ttu-id="0421b-129">Adja meg az a virtuális gép részleteit, mivel válassza ki az első lépésben létrehozott felhőalapú szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="0421b-129">As you fill out the VM details, choose the name of cloud service created in the first step.</span></span>

  ![A virtuális gép egy felhőalapú szolgáltatás kiválasztása](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
