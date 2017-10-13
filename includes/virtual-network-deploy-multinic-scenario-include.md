## <a name="scenario"></a><span data-ttu-id="de60f-101">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="de60f-101">Scenario</span></span>
<span data-ttu-id="de60f-102">Jelen dokumentum részletesen ismerteti a használó központi telepítések, több hálózati adaptert a virtuális gépek az adott forgatókönyv keresztül.</span><span class="sxs-lookup"><span data-stu-id="de60f-102">This document will walk through a deployment that uses multiple NICs in VMs in a specific scenario.</span></span> <span data-ttu-id="de60f-103">Ebben a forgatókönyvben a kétféle IaaS munkaterhelések Azure-ban üzemeltetett rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="de60f-103">In this scenario, you have a two-tiered IaaS workload hosted in Azure.</span></span> <span data-ttu-id="de60f-104">Minden egyes réteg a virtuális hálózatot (VNet) a saját alhálózat lett telepítve.</span><span class="sxs-lookup"><span data-stu-id="de60f-104">Each tier is deployed in its own subnet in a virtual network (VNet).</span></span> <span data-ttu-id="de60f-105">több webkiszolgálók csoportosítva beállítása a magas rendelkezésre állású terheléselosztó hello előtér-rétegből állnak.</span><span class="sxs-lookup"><span data-stu-id="de60f-105">hello front end tier is composed of several web servers, grouped together in a load balancer set for high availability.</span></span> <span data-ttu-id="de60f-106">hello háttér réteg több adatbázis-kiszolgálók tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="de60f-106">hello back end tier is composed of several database servers.</span></span> <span data-ttu-id="de60f-107">Ezek az adatbázis-kiszolgálók két hálózati adapterrel rendelkező minden, egy adatbázis-hozzáférési telepíti, hello más felügyeleti.</span><span class="sxs-lookup"><span data-stu-id="de60f-107">These database servers will be deployed with two NICs each, one for database access, hello other for management.</span></span> <span data-ttu-id="de60f-108">hello eset is tartalmazza a hálózati biztonsági csoportokkal (NSG-k) toocontrol hello telepítési forgalom engedélyezett tooeach alhálózatot, és a hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="de60f-108">hello scenario also includes Network Security Groups (NSGs) toocontrol what traffic is allowed tooeach subnet, and NIC in hello deployment.</span></span> <span data-ttu-id="de60f-109">hello az alábbi ábra hello alapvető architektúráját ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="de60f-109">hello figure below shows hello basic architecture of this scenario.</span></span>  

![MultiNIC forgatókönyv](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)
